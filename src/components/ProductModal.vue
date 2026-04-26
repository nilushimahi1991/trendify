<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4"
    @click.self="$emit('close')">
    <div class="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
      
      <!-- Close Button -->
      <div class="flex justify-end p-4">
        <button @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 text-2xl font-bold">✕</button>
      </div>

      <!-- Product Image -->
      <img
        :src="product.thumbnail"
        :alt="product.title"
        class="w-full h-64 object-cover"
        @error="(e) => (e.target as HTMLImageElement).src = 'https://placehold.co/400x300?text=No+Image'"
      />

      <!-- Product Details -->
      <div class="p-6">
        <span class="text-purple-500 text-sm font-medium uppercase">{{ product.category }}</span>
        <h2 class="text-2xl font-bold text-gray-800 mt-1">{{ product.title }}</h2>
        <p class="text-gray-500 mt-3 leading-relaxed">{{ product.description }}</p>

        <div class="flex items-center gap-4 mt-4">
          <span class="text-3xl font-bold text-purple-600">${{ product.price }}</span>
          <span class="text-yellow-400">⭐ {{ product.rating }}</span>
          <span class="text-green-500 text-sm font-medium">{{ product.stock }} in stock</span>
        </div>

       <button
  @click="addToCart"
  class="mt-6 w-full bg-purple-600 text-white py-3 rounded-xl hover:bg-purple-700 transition font-semibold text-lg">
 Add to Cart Now 🛒
</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useCartStore } from '../stores/cartStore'

interface Product {
  id: number
  title: string
  price: number
  rating: number
  thumbnail: string
  category: string
  description: string
  stock: number
}

const props = defineProps<{ product: Product }>()
defineEmits<{ close: [] }>()

const cart = useCartStore()

const addToCart = () => {
  cart.addToCart({
    id: props.product.id,
    title: props.product.title,
    price: props.product.price,
    thumbnail: props.product.thumbnail,
    quantity: 1
  })
}
</script>