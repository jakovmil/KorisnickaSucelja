<template>
  <div class="group cursor-pointer" @click="open">
    <div class="relative aspect-[2/3] w-full overflow-hidden rounded-[4px] transition-all duration-300 hover:scale-105 hover:shadow-2xl hover:z-50">
      <img :src="film.slika" class="w-full h-full object-cover" />
    </div>

    <div class="mt-3">
      <h3 class="font-bold text-gray-100 text-[15px] truncate">{{ film.naslov }}</h3>
      <div class="flex items-center gap-2 mt-1 text-[11px] font-bold text-gray-400">
        <span class="text-[#46d369]">{{ film.match }} Match</span>
        <span>{{ film.god }}</span>
        <span class="border border-gray-500 rounded px-1 text-[9px] text-gray-400">HD</span>
        <span class="border border-gray-500 rounded px-1 text-[9px] text-gray-400">5.1</span>
      </div>
      <div class="flex items-center gap-2 mt-1">
         <span 
           class="border border-gray-600 px-1.5 text-[10px] font-bold text-white bg-transparent"
           :class="{'bg-[#e50914] border-none': film.dob === '18', 'bg-[#d97706] border-none': film.dob === '12'}"
         >
           {{ film.dob }}
         </span>
         <span class="text-[11px] text-gray-500">Crime • Thriller</span>
      </div>
    </div>
  </div>
</template>

<script setup>
//defineProps
const props = defineProps(['film']);
const emit = defineEmits(['odabran']);

function open() {
  //Emit event za parent komponente
  emit('odabran', props.film.naslov);

  //koristimo props.film za new tab
  const query = encodeURIComponent(`${props.film.naslov}`);
  const url = `https://www.imdb.com/find?q=${query}`;
  window.open(url, '_blank', 'noopener,noreferrer');
}
</script>