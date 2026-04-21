<template>
  <!-- Overlay -->
  <div class="fixed inset-0 bg-black bg-opacity-50 z-50 flex justify-end"
    @click.self="$emit('close')">
    
    <!-- Sidebar -->
    <div class="bg-white w-full max-w-md h-full flex flex-col shadow-2xl">
      
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b">
        <h2 class="text-2xl font-bold text-gray-800">Your Cart 🛒</h2>
        <div class="flex items-center gap-2 mt-1">
  <button @click="cart.removeFromCart(item.id)"
    class="text-red-400 hover:text-red-600 text-sm px-2 py-1 border border-red-300 rounded-lg">
    Remove 🗑️
  </button>
</div>
      </div>

      <!-- Empty Cart -->
      <div v-if="cart.items.length === 0"
        class="flex-1 flex flex-col items-center justify-center text-gray-400">
        <span class="text-6xl mb-4">🛒</span>
        <p class="text-xl">Your cart is empty!</p>
      </div>

      <!-- Cart Items -->
      <div v-else class="flex-1 overflow-y-auto p-6 space-y-4">
        <div v-for="item in cart.items" :key="item.id"
          class="flex items-center gap-4 bg-gray-50 rounded-xl p-3">
          <img :src="item.thumbnail" :alt="item.title"
            class="w-16 h-16 object-cover rounded-lg" />
          <div class="flex-1">
            <h3 class="font-semibold text-gray-800 text-sm truncate">{{ item.title }}</h3>
            <p class="text-purple-600 font-bold">${{ item.price }}</p>
            <p class="text-gray-400 text-sm">Qty: {{ item.quantity }}</p>
          </div>
          <button @click="cart.removeFromCart(item.id)"
            class="text-red-400 hover:text-red-600 text-xl">🗑️</button>
        </div>
      </div>

      <!-- Footer -->
      <div v-if="cart.items.length > 0" class="p-6 border-t">
        <div class="flex justify-between text-lg font-bold text-gray-800 mb-4">
          <span>Total:</span>
          <span class="text-purple-600">${{ cart.totalPrice }}</span>
        </div>
        <button
          class="w-full bg-purple-600 text-white py-3 rounded-xl hover:bg-purple-700 transition font-semibold text-lg">
          Checkout 💳
        </button>
        <button @click="cart.clearCart()"
          class="w-full mt-2 border border-red-400 text-red-400 py-3 rounded-xl hover:bg-red-50 transition font-medium">
          Clear Cart 🗑️
        </button>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { useCartStore } from '../stores/cartStore'
const cart = useCartStore()
defineEmits<{ close: [] }>()
</script>