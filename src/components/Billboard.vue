<template>
  <div class="relative h-[45vw] md:h-[40vw] max-h-[550px] w-full bg-black overflow-hidden">
    
    <transition name="fade">
      <div :key="currentIndex" class="absolute inset-0">
        <img 
          v-if="aktivniFilm"
          :src="aktivniFilm.slika" 
          class="w-full h-full object-cover brightness-[55%]" 
          :alt="aktivniFilm.naslov"
        />
        
        <div class="absolute top-[25%] md:top-[30%] ml-4 md:ml-12 z-10 max-w-[90%] md:max-w-[45%]">
          <h1 class="text-white text-2xl md:text-5xl lg:text-6xl font-extrabold drop-shadow-2xl transition-all duration-500">
            {{ aktivniFilm?.naslov }}
          </h1>

          <p class="text-white text-[10px] md:text-base lg:text-lg mt-3 md:mt-5 drop-shadow-md leading-relaxed font-medium">
            Otkrijte čarobnu priču o prijateljstvu, hrabrosti i nevjerojatnim avanturama u svijetu gdje se mašta i stvarnost isprepliću. Bezvremensko remek-djelo koje osvaja srca svih generacija.
          </p>
          
          <div class="flex items-center gap-3 mt-4 md:mt-8">
            <button 
              @click="otvoriFilm"
              class="bg-white text-black px-4 py-1.5 md:px-7 md:py-2.5 rounded font-bold hover:bg-gray-200 transition flex items-center gap-2 text-sm md:text-lg"
            >
               Reproduciraj
            </button>
            <button 
              class="bg-gray-500/50 text-white px-4 py-1.5 md:px-7 md:py-2.5 rounded font-bold hover:bg-gray-500/70 transition text-sm md:text-lg"
            >
              ⓘ Više informacija
            </button>
          </div>
        </div>
      </div>
    </transition>

    <div class="absolute bottom-0 w-full h-24 md:h-32 bg-gradient-to-t from-[#141414] to-transparent"></div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  filmovi: {
    type: Array,
    required: true
  }
});

const currentIndex = ref(0);
let timer = null;

const aktivniFilm = computed(() => {
  if (props.filmovi.length === 0) return null;
  return props.filmovi[currentIndex.value];
});

const sljedeciFilm = () => {
  if (props.filmovi.length > 0) {
    currentIndex.value = (currentIndex.value + 1) % props.filmovi.length;
  }
};

const otvoriFilm = () => {
  if (!aktivniFilm.value) return;
  const query = encodeURIComponent(aktivniFilm.value.naslov);
  const url = `https://www.imdb.com/find?q=${query}`;
  window.open(url, '_blank', 'noopener,noreferrer');
};

onMounted(() => {
  timer = setInterval(sljedeciFilm, 3000);
});

onUnmounted(() => {
  if (timer) clearInterval(timer);
});
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.2s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>