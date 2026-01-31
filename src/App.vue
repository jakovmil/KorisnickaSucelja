<script setup>
import { ref, onMounted, computed } from 'vue';
import MovieCard from './components/MovieCard.vue';

const pretraga = ref('');
const status = ref('Povezivanje s API serverom...');
const filmovi = ref([]); 

// === PRAVI API POZIV (HTTPS) ===
const dohvatiPodatkeSaInterneta = async () => {
  try {
    const response = await fetch('https://ghibliapi.vercel.app/films');

    if (!response.ok) {
      throw new Error('API nije odgovorio');
    }

    const data = await response.json();

    filmovi.value = data.slice(0, 18).map(f => ({
      id: f.id,
      naslov: f.title,
      slika: f.image,       // već full https slika
      match: '95%',
      god: f.release_date,
      dob: '12'
    }));

    status.value = 'Besplatan API učitan (HTTPS, bez ključa)';
  } catch (err) {
    status.value = 'GREŠKA: API se nije učitao';
    console.error(err);
  }
};




// Pokreni API čim se aplikacija učita
onMounted(() => {
  dohvatiPodatkeSaInterneta();
});

// LOGIKA PRETRAGE
const filtriraniFilmovi = computed(() => {
  return filmovi.value.filter(f => 
    f.naslov.toLowerCase().includes(pretraga.value.toLowerCase())
  );
});

// EMIT HANDLER
const obradiKlik = (naslov) => {
  status.value = "Odabrali ste: " + naslov;
};
</script>

<template>
  <div class="bg-[#141414] min-h-screen min-w-[1200px] text-white font-sans overflow-x-auto">
    
    <nav class="sticky top-0 z-50 bg-[#141414] px-12 py-5 flex items-center justify-between shadow-lg">
      <div class="flex items-center gap-10">
        <h1 class="text-[#E50914] text-3xl font-bold tracking-tighter cursor-pointer">MINI NETFLIX</h1>
      </div>

      <div class="flex items-center gap-6">
        <div class="relative flex items-center border border-white/20 px-3 py-1 bg-black">
           <span class="mr-2">🔍</span>
           <input v-model="pretraga" type="text" placeholder="Pretraži crtani..." class="bg-transparent text-white text-sm w-[250px] focus:outline-none">
        </div>
      </div>
    </nav>

    <main class="px-12 pt-8 pb-20">
      <div class="mb-6 border-b border-gray-800 pb-4">
        <h2 class="text-4xl font-bold mb-2">Popular Cartoon</h2>
        <p class="text-blue-400 text-xs font-bold uppercase tracking-[0.2em]">{{ status }}</p>
      </div>

      <div v-if="filmovi.length === 0" class="flex justify-center mt-20">
         <div class="animate-pulse text-xl">Učitavanje sa HTTPS servera...</div>
      </div>

      <div v-else class="grid grid-cols-6 gap-x-4 gap-y-12">
        <MovieCard 
          v-for="film in filtriraniFilmovi" 
          :key="film.id" 
          :film="film" 
          @odabran="obradiKlik"
        />
      </div>
    </main>
  </div>
</template>