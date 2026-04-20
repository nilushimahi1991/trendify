<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-800 transition-colors duration-300">
    <NavBar
      :user="currentUser"
      @openCart="cartOpen = true"
      @openLogin="loginOpen = true"
    />

    <main class="max-w-7xl mx-auto px-4 py-8">
      <h2 class="text-3xl font-bold text-red-500 mb-6">New Arrivals 🔥</h2>

      <FilterBar
        :categories="categories"
        @search="onSearch"
        @filter="onFilter"
      />

      <div v-if="loading" class="text-center py-20 text-purple-600 text-xl">
        Loading... ⏳
      </div>

      <div v-else-if="filteredProducts.length === 0"
        class="text-center py-20 text-gray-400 text-xl">
        No products found 😔
      </div>

      <div v-else class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <ProductCard
          v-for="product in filteredProducts"
          :key="product.id"
          :product="product"
          @click="openModal(product)"
        />
      </div>
    </main>

    <!-- Product Modal -->
    <ProductModal
      v-if="selectedProduct"
      :product="selectedProduct"
      @close="selectedProduct = null"
    />

    <!-- Cart Sidebar -->
    <CartSidebar
      v-if="cartOpen"
      @close="cartOpen = false"
    />

    <!-- Login Modal -->
    <LoginModal
      v-if="loginOpen"
      @close="loginOpen = false"
      @loggedIn="onLoggedIn"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import NavBar from './components/NavBar.vue'
import ProductCard from './components/ProductCard.vue'
import FilterBar from './components/FilterBar.vue'
import ProductModal from './components/ProductModal.vue'
import CartSidebar from './components/CartSidebar.vue'
import LoginModal from './components/LoginModal.vue'
import { useCartStore } from './stores/cartStore'

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

const cart = useCartStore()
const products = ref<Product[]>([])
const loading = ref(true)
const searchQuery = ref('')
const selectedCategory = ref('')
const selectedProduct = ref<Product | null>(null)
const cartOpen = ref(false)
const loginOpen = ref(false)
const storedUser = localStorage.getItem('user')
const currentUser = ref<string | null>(
  storedUser && !storedUser.startsWith('{') ? storedUser : null
)

const categories = computed(() => {
  const cats = products.value.map(p => p.category)
  return [...new Set(cats)]
})

const filteredProducts = computed(() => {
  return products.value.filter(p => {
    const matchSearch = p.title.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchCategory = selectedCategory.value === '' || p.category === selectedCategory.value
    return matchSearch && matchCategory
  })
})

const onSearch = (query: string) => searchQuery.value = query
const onFilter = (category: string) => selectedCategory.value = category
const openModal = (product: Product) => selectedProduct.value = product

const onLoggedIn = (name: string, token: string) => {
  currentUser.value = name
  loginOpen.value = false
}

onMounted(async () => {
  const res = await fetch('https://dummyjson.com/products?limit=150')
  const data = await res.json()
  products.value = data.products
  loading.value = false
})
</script>