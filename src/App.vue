<template>
  <div class="min-h-screen bg-purple-400 dark:bg-gray-800 transition-colors duration-300">
    
    <!-- Login Page -->
    <div v-if="!currentUser" class="min-h-screen flex items-center justify-center">
      <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md p-8">
        <div class="text-center mb-6">
          <h1 class="text-3xl font-bold text-purple-700">Trendify 👗</h1>
          <p class="text-gray-400 mt-2">Please login to continue</p>
        </div>

        <div v-if="loginError" class="bg-red-100 text-red-600 px-4 py-3 rounded-xl mb-4 text-sm">
          {{ loginError }}
        </div>

        <div class="space-y-4">
          <div>
            <label class="block text-gray-600 text-sm font-medium mb-1">Username</label>
            <input
              v-model="loginUsername"
              type="text"
              placeholder="Enter username"
              class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-purple-400"
            />
          </div>
          <div>
            <label class="block text-gray-600 text-sm font-medium mb-1">Password</label>
            <input
              v-model="loginPassword"
              type="password"
              placeholder="Enter password"
              class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-purple-400"
            />
          </div>
          <button
            @click="handleLogin"
            :disabled="loginLoading"
            class="w-full bg-purple-600 text-white py-3 rounded-xl hover:bg-purple-700 transition font-semibold text-lg disabled:opacity-50">
            {{ loginLoading ? 'Logging in...' : 'Login' }}
          </button>
          <p class="text-center text-gray-400 text-sm">
            Test: <span class="text-purple-500 font-medium">emilys</span> / <span class="text-purple-500 font-medium">emilyspass</span>
          </p>
        </div>
      </div>
    </div>

    <!-- Main App -->
    <div v-else>
      <NavBar
        :user="currentUser"
        @openCart="cartOpen = true"
        @openLogin="loginOpen = true"
      />

      <main id="top" class="max-w-7xl mx-auto px-4 py-8">
        <div class="relative w-full h-64 md:h-96 rounded-2xl overflow-hidden mb-10">
          <img src="/banner.jpg" alt="Trendify Banner" class="w-full h-full object-cover" />
          <div class="absolute inset-0 bg-black bg-opacity-40 flex flex-col items-center justify-center text-white text-center px-4">
            <h1 class="text-4xl md:text-6xl font-bold mb-4">Welcome to Trendify</h1>
            <p class="text-lg md:text-2xl mb-6">Trendy Picks Just for You</p>
            <button @click="scrollToProducts"
              class="bg-purple-600 hover:bg-purple-700 text-white px-8 py-3 rounded-full font-semibold text-lg transition cursor-pointer">
              Shop Now 🛍️
            </button>
          </div>
        </div>

        <div id="products"></div>

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
            <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
          </select>
        </div>

        <div v-if="loading" class="text-center py-20 text-purple-600 text-xl">
          Loading... ⏳
        </div>

        <div v-else-if="filteredProducts.length === 0" class="text-center py-20 text-gray-400 text-xl">
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

      <ProductModal v-if="selectedProduct" :product="selectedProduct" @close="selectedProduct = null" />
      <CartSidebar v-if="cartOpen" @close="cartOpen = false" />
      <LoginModal v-if="loginOpen" @close="loginOpen = false" @loggedIn="onLoggedIn" />
      <Footer />
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import NavBar from './components/NavBar.vue'
import ProductCard from './components/ProductCard.vue'
import ProductModal from './components/ProductModal.vue'
import CartSidebar from './components/CartSidebar.vue'
import LoginModal from './components/LoginModal.vue'
import { useCartStore } from './stores/cartStore'
import Footer from './components/Footer.vue'

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

const loginUsername = ref('')
const loginPassword = ref('')
const loginLoading = ref(false)
const loginError = ref('')

const scrollToProducts = () => {
  const el = document.getElementById('products')
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}

const handleLogin = async () => {
  if (!loginUsername.value || !loginPassword.value) {
    loginError.value = 'Please enter username and password!'
    return
  }
  loginLoading.value = true
  loginError.value = ''
  try {
    const res = await fetch('https://dummyjson.com/user/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: loginUsername.value,
        password: loginPassword.value,
        expiresInMins: 30
      })
    })
    const data = await res.json()
    if (data.accessToken) {
      localStorage.setItem('token', data.accessToken)
      localStorage.setItem('user', 'Mahi Yapa')
      currentUser.value = 'Mahi Yapa'
    } else {
      loginError.value = 'Invalid username or password!'
    }
  } catch {
    loginError.value = 'Something went wrong. Try again!'
  } finally {
    loginLoading.value = false
  }
}

const categories = computed(() => {
  const cats = products.value.map(p => p.category)
  return [...new Set(cats)]
})

const filteredProducts = computed(() => {
  return products.value.filter(p => {
    const matchSearch = searchQuery.value === '' ||
      p.title.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      p.category.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchCategory = selectedCategory.value === '' || p.category === selectedCategory.value
    return matchSearch && matchCategory
  })
})

const openModal = (product: Product) => selectedProduct.value = product

const onLoggedIn = (name: string, token: string) => {
  currentUser.value = name
  loginOpen.value = false
}

onMounted(async () => {
  const cats = ['beauty', 'fragrances', 'home-decoration', 'mens-watches', 'skin-care']
  const results = await Promise.all(
    cats.map(cat =>
      fetch(`https://dummyjson.com/products/category/${cat}?limit=5`).then(r => r.json())
    )
  )
  products.value = results.flatMap(r => r.products)
  loading.value = false
})
</script>