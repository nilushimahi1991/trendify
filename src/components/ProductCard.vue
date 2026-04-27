<template>
  <div class="bg-gray-400 rounded-2xl shadow-md hover:shadow-xl transition-shadow duration-300 cursor-pointer overflow-hidden">
    <!-- Product Image -->
   <img
  :src="product.thumbnail"
  :alt="product.title"
  class="w-full h-56 object-cover hover:scale-105 transition-transform duration-300"
  @error="(e) => {
    const target = e.target as HTMLImageElement;
    target.src = '';
    target.style.display = 'none';
    target.parentElement!.innerHTML += '<div class=\'w-full h-56 bg-gray-100 flex items-center justify-center\'><span class=\'text-gray-400 text-lg\'>📷 No Image</span></div>';
  }"
/>
    <!-- Product Info -->
    <div class="p-4">
      <h3 class="font-semibold text-gray-800 text-lg truncate">{{ product.title }}</h3>
      <p class="text-gray-400 text-sm mt-1 truncate">{{ product.category }}</p>

      <div class="flex items-center justify-between mt-3">
        <span class="text-purple-600 font-bold text-xl">${{ product.price }}</span>
        <span class="text-yellow-400 text-sm">⭐ {{ product.rating }}</span>
      </div>

      <button class="mt-4 w-full bg-purple-600 text-white py-2 rounded-xl hover:bg-purple-700 transition font-medium">
        Add to Cart 🛒
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Product {
  id: number
  title: string
  price: number
  rating: number
  thumbnail: string
  category: string
}

defineProps<{ product: Product }>()
</script>