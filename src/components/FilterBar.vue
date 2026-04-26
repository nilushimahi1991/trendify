<template>
  <div class="flex flex-col sm:flex-row gap-4 mb-8">
    <input
      v-model="searchQuery"
      type="text"
      placeholder="Search products... 🔍"
      style="background-color: #9ca3af;"
      class="flex-1 px-4 py-3 rounded-xl border border-gray-200 shadow-sm focus:outline-none focus:ring-2 focus:ring-purple-400 text-gray-700 placeholder-black"
    />

    <select
      v-model="selectedCategory"
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
import { ref, watch } from 'vue'

const props = defineProps<{ categories: string[] }>()
const emit = defineEmits<{
  (e: 'search', query: string): void
  (e: 'filter', category: string): void
}>()

const searchQuery = ref('')
const selectedCategory = ref('')

watch(searchQuery, (val) => {
  emit('search', val)
})

watch(selectedCategory, (val) => {
  emit('filter', val)
})
</script>