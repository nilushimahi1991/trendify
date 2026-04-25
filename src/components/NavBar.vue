<template>
  <nav class="bg-blue-900 dark:bg-gray-900 shadow-md px-6 py-4 flex items-center justify-between transition-colors duration-300">
    <!-- Logo -->
   <div class="flex items-center gap-2">
  <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-white" viewBox="0 0 24 24" fill="currentColor">
    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 15v-4H7l5-8v4h4l-5 8z"/>
  </svg>
  <span class="text-2xl font-bold text-white tracking-wide">Trendify</span>
</div>

    <!-- Nav Links -->
    <div class="hidden md:flex gap-6 text-white dark:text-gray-300 font-medium">
  <a href="#top" class="hover:text-purple-600 transition">Home</a>
  
  <a href="#about" class="hover:text-purple-600 transition">About</a>
</div>

    <!-- Right Side -->
    <div class="flex items-center gap-4">
      <!-- Dark Mode -->
      <button @click="toggleDark" class="text-2xl hover:scale-110 transition-transform">
        {{ isDark ? '☀️' : '🌙' }}
      </button>

      <!-- Login/Logout -->
      <div v-if="user">
        <span class="text-white dark:text-gray-300 text-sm font-medium mr-2">{{ user }}</span>
        <button @click="logout"
          class="bg-red-100 text-red-500 px-3 py-1 rounded-lg text-sm hover:bg-red-200 transition">
          Logout
        </button>
      </div>
      <button v-else @click="$emit('openLogin')"
        class="bg-purple-600 text-white px-4 py-2 rounded-xl hover:bg-purple-700 transition text-sm font-medium">
        Login 🔐
      </button>

      <!-- Cart -->
      <button @click="$emit('openCart')"
        class="relative text-gray-600 dark:text-gray-300 hover:text-purple-600 transition text-2xl">
        🛒
        <span v-if="cart.totalItems > 0"
          class="absolute -top-2 -right-2 bg-purple-600 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
          {{ cart.totalItems }}
        </span>
      </button>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useCartStore } from '../stores/cartStore'

const cart = useCartStore()

defineProps<{ user: string | null }>()
defineEmits<{
  openCart: []
  openLogin: []
}>()

const isDark = ref(false)

const toggleDark = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}

const logout = () => {
  localStorage.removeItem('token')
  localStorage.removeItem('user')
  location.reload()
}
</script>