<script setup>
defineEmits(['back']);

// Mock user data
const user = {
  name: 'Ghibli Fan',
  email: 'user@example.com',
  plan: 'Premium Ultra HD',
  memberSince: 'Veljača 2026'
};
</script>

<template>
  <div class="max-w-2xl mx-auto pt-10 px-4">
    <div class="flex items-center gap-4 mb-8">
      <button @click="$emit('back')" class="text-gray-400 hover:text-white flex items-center gap-2">
        <span>←</span> Natrag na početnu
      </button>
    </div>

    <h1 class="text-5xl font-bold mb-8">Račun</h1>
    
    <div class="border-t border-gray-700 py-6 grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="text-gray-500 font-bold uppercase text-sm">Članstvo i naplata</div>
      <div class="md:col-span-2 flex flex-col gap-2">
        <p class="font-bold">{{ user.email }}</p>
        <p class="text-gray-400">Lozinka: ********</p>
        <p class="text-gray-400">Član od: {{ user.memberSince }}</p>
      </div>
    </div>

    <div class="border-t border-gray-700 py-6 grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="text-gray-500 font-bold uppercase text-sm">Podaci o planu</div>
      <div class="md:col-span-2 flex items-center justify-between">
        <p class="font-bold">{{ user.plan }}</p>
        <button class="text-blue-500 hover:underline">Promijeni plan</button>
      </div>
    </div>

    <button class="w-full mt-10 py-3 border border-gray-600 text-gray-300 font-bold hover:bg-gray-800 transition">
      Otkaži članstvo
    </button>
  </div>
</template>