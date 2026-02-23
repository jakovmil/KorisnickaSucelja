<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['login-success']);

const email = ref('');
const password = ref('');

const handleLogin = () => {
  if (email.value && password.value) {
    emit('login-success');
  }
};
</script>

<template>
  <div class="relative min-h-screen w-full bg-black flex items-center justify-center font-sans">

    <div class="relative z-10 w-full max-w-[450px] bg-black/75 p-8 md:p-16 rounded-md">
      <h2 class="text-white text-3xl font-bold mb-8">Prijava</h2>

      <form @submit.prevent="handleLogin" class="flex flex-col gap-4">
        <div class="relative">
          <input 
            v-model="email"
            type="email" 
            placeholder="Email ili broj telefona"
            class="w-full bg-[#333] text-white px-5 py-4 rounded focus:outline-none focus:bg-[#454545] transition-colors"
          >
        </div>

        <div class="relative">
          <input 
            v-model="password"
            type="password" 
            placeholder="Lozinka"
            class="w-full bg-[#333] text-white px-5 py-4 rounded focus:outline-none focus:bg-[#454545] transition-colors"
          >
        </div>

        <p v-if="error" class="text-[#E87C03] text-sm">{{ error }}</p>

        <button 
          type="submit"
          class="bg-[#E50914] text-white font-bold py-4 rounded mt-6 hover:bg-[#c10712] transition-colors"
        >
          Prijavi se
        </button>

        <div class="flex items-center justify-between text-[#b3b3b3] text-xs mt-2">
          <label class="flex items-center gap-1 cursor-pointer">
            <input type="checkbox" class="accent-gray-500"> Zapamti me
          </label>
          <a href="#" class="hover:underline">Trebate pomoć?</a>
        </div>
      </form>

      <div class="mt-16 text-[#737373]">
        <p>Novi na Mini Netflixu? <a href="#" class="text-white hover:underline">Registriraj se.</a></p>
        <p class="text-sm mt-4">Ova stranica je zaštićena Google reCAPTCHA sustavom.</p>
      </div>
    </div>
  </div>
</template>