<template>
  <nav class="bg-white dark:bg-gray-900 shadow-md px-6 py-4 flex items-center justify-between transition-colors duration-300">
    <!-- Logo -->
    <div class="text-2xl font-bold text-purple-600">
      Trendify 👗
    </div>

    <!-- Nav Links -->
    <div class="hidden md:flex gap-6 text-gray-600 dark:text-gray-300 font-medium">
      <a href="#" class="hover:text-purple-600 transition">Home</a>
      <a href="#" class="hover:text-purple-600 transition">Shop</a>
      <a href="#" class="hover:text-purple-600 transition">About</a>
    </div>

    <!-- Right Side -->
    <div class="flex items-center gap-4">
      <!-- Dark Mode Toggle -->
      <button @click="toggleDark"
        class="text-2xl hover:scale-110 transition-transform">
        {{ isDark ? '☀️' : '🌙' }}
      </button>

      <!-- Cart Icon -->
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
defineEmits<{ openCart: [] }>()

const isDark = ref(false)

const toggleDark = () => {
  isDark.value = !isDark.value
  if (isDark.value) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
}
</script>