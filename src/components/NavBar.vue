<template>
  <nav class="bg-white dark:bg-gray-900 shadow-md px-6 py-4 flex items-center justify-between transition-colors duration-300">
    <!-- Logo -->
    <div class="text-2xl font-bold text-purple-700 tracking-wide">
      Trendify 👗
    </div>

    <!-- Nav Links -->
    <div class="hidden md:flex gap-6 text-gray-600 dark:text-gray-300 font-medium">
  <a href="#top" class="hover:text-purple-600 transition">Home</a>
  <a href="#products" class="hover:text-purple-600 transition">Shop</a>
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
        <span class="text-gray-600 dark:text-gray-300 text-sm font-medium mr-2">👋 {{ user }}</span>
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