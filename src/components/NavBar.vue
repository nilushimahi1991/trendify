<template>
  <nav class="bg-white shadow-md px-6 py-4 flex items-center justify-between">
    <!-- Logo -->
    <div class="text-2xl font-bold text-purple-600">
      Trendify 👗
    </div>

    <!-- Nav Links -->
    <div class="hidden md:flex gap-6 text-gray-600 font-medium">
      <a href="#" class="hover:text-purple-600 transition">Home</a>
      <a href="#" class="hover:text-purple-600 transition">Shop</a>
      <a href="#" class="hover:text-purple-600 transition">About</a>
    </div>

    <!-- Cart Icon -->
    <div class="flex items-center gap-4">
      <button @click="$emit('openCart')"
        class="relative text-gray-600 hover:text-purple-600 transition text-2xl">
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
import { useCartStore } from '../stores/cartStore'
const cart = useCartStore()
defineEmits<{ openCart: [] }>()
</script>