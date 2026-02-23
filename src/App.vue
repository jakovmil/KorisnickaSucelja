<script setup>
import { ref, onMounted, computed } from 'vue';
import MovieCard from './components/MovieCard.vue';
import Login from './components/Login.vue';
import Profile from './components/Profile.vue';
import Billboard from './components/Billboard.vue';

//App
const isLoggedin = ref(false);
const pretraga = ref('');
const status = ref('Povezivanje s API serverom...');
const filmovi = ref([]); 
const view = ref('home');

//Auth
const handleLogout = () => {
  isLoggedin.value = false;
  view.value = 'home';
};

//API hvacanje
const dohvatiPodatkeSaInterneta = async () => {
  try {
    const response = await fetch('https://ghibliapi.vercel.app/films');
    if (!response.ok) throw new Error('API nije odgovorio');
    const data = await response.json();

    filmovi.value = data.slice(0, 18).map(f => ({
      id: f.id,
      naslov: f.title,
      slika: f.image,
      match: '95%',
      god: f.release_date,
      dob: '12'
    }));
    status.value = 'API ucitan';
  } catch (err) {
    status.value = 'GREŠKA: API se nije učitao';
    console.error(err);
  }
};

onMounted(dohvatiPodatkeSaInterneta);

const filtriraniFilmovi = computed(() => {
  return filmovi.value.filter(f => 
    f.naslov.toLowerCase().includes(pretraga.value.toLowerCase())
  );
});

const obradiKlik = (naslov) => {
  status.value = "Odabrali ste: " + naslov;
};
</script>

<template>
  <Login v-if="!isLoggedin" @login-success="isLoggedin = true" />

  <div v-else class="bg-[#141414] min-h-screen w-full text-white font-sans">
    
    <nav class="sticky top-0 z-50 bg-[#141414] px-4 md:px-12 py-4 flex items-center justify-between shadow-lg">
      <div class="flex items-center gap-8">
        <h1 @click="view = 'home'" class="text-[#E50914] text-2xl md:text-3xl font-bold tracking-tighter cursor-pointer">
          MINI NETFLIX
        </h1>
        <button @click="view = 'home'" :class="view === 'home' ? 'text-white' : 'text-gray-400'" class="hidden md:block text-sm font-medium">
          Početna
        </button>
      </div>

      <div class="flex items-center gap-4 sm:gap-6 ml-auto">
        <div v-if="view === 'home'" class="relative flex items-center border border-white/20 px-3 py-1 bg-black sm:flex">
           <span class="mr-2 text-xs"></span>
           <input v-model="pretraga" type="text" placeholder="Pretraži..." class="bg-transparent text-white text-sm w-[150px] md:w-[250px] focus:outline-none">
        </div>

        <button @click="view = 'profile'" class="flex items-center gap-2 group p-1 border border-transparent rounded-sm transition-all">
          <img 
            src="./assets/userpic.png" 
            alt="Profile" 
            class="w-8 h-8 object-cover rounded-sm"
          />
          <span class="text-sm hidden sm:inline" :class="view === 'profile' ? 'font-bold' : ''">Profil</span>
        </button>

        <button @click="handleLogout" class="text-white text-sm font-medium hover:text-gray-300 transition border-l border-gray-700 pl-4">
          Odjavi se
        </button>
      </div>
    </nav>

    <main v-if="view === 'home'">
      <Billboard :filmovi="filmovi" />

      <div class="px-4 md:px-12 pt-10 pb-20">
        <div class="mb-6 border-b border-gray-800 pb-4">
          <h2 class="text-2xl md:text-4xl font-bold mb-2">Popular Cartoon</h2>
          <p class="text-blue-400 text-[10px] md:text-xs font-bold uppercase tracking-[0.2em]">{{ status }}</p>
        </div>

        <div v-if="filmovi.length === 0" class="flex justify-center mt-20">
           <div class="animate-pulse text-lg md:text-xl">Učitavanje...</div>
        </div>
        
        <div v-else-if="filtriraniFilmovi.length === 0" class="text-center py-20 text-gray-500">
            Nema rezultata za: "{{ pretraga }}"
        </div>

        <div v-else class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-x-3 gap-y-8 md:gap-x-4 md:gap-y-12">
          <MovieCard 
            v-for="film in filtriraniFilmovi" 
            :key="film.id" 
            :film="film" 
            @odabran="obradiKlik"
          />
        </div>
      </div>
    </main>

    <main v-else-if="view === 'profile'" class="px-4 md:px-12">
      <Profile @back="view = 'home'" />
    </main>

  </div>
</template>