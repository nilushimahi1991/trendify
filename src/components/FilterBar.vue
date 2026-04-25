<template>
  <div class="flex flex-col sm:flex-row gap-4 mb-8">
    <!-- Search Bar -->
    <input
      v-model="searchQuery"
      @input="$emit('search', searchQuery)"
      type="text"
      placeholder="Search products... 🔍"
   class="flex-1 px-4 py-3 rounded-xl border border-gray-200 shadow-sm focus:outline-none focus:ring-2 focus:ring-purple-400 text-gray-700 placeholder-black bg-gray-400"
    />

    <!-- Category Filter -->
    <select
  v-model="selectedCategory"
  @change="$emit('filter', selectedCategory)"
  style="background-color: #9ca3af; color: black;"
  class="px-4 py-3 rounded-xl border border-gray-300 shadow-sm focus:outline-none focus:ring-2 focus:ring-purple-400">
      <option value="">All Categories</option>
      <option v-for="cat in categories" :key="cat" :value="cat">
        {{ cat }}
      </option>
    </select>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ categories: string[] }>()
defineEmits<{
  search: [query: string]
  filter: [category: string]
}>()

const searchQuery = ref('')
const selectedCategory = ref('')
</script>