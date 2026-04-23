<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4"
    @click.self="$emit('close')">
    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-2xl w-full max-w-md p-8">
      
      <!-- Header -->
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-2xl font-bold text-gray-800 dark:text-white">Login 🔐</h2>
        <button @click="$emit('close')" class="text-gray-400 hover:text-gray-600 text-2xl">✕</button>
      </div>

      <!-- Error Message -->
      <div v-if="error" class="bg-red-100 text-red-600 px-4 py-3 rounded-xl mb-4 text-sm">
        {{ error }}
      </div>

      <!-- Form -->
      <div class="space-y-4">
        <div>
          <label class="block text-gray-600 dark:text-gray-300 text-sm font-medium mb-1">Username</label>
          <input
            v-model="username"
            type="text"
            placeholder="Enter username"
            class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-purple-400 dark:bg-gray-700 dark:text-white dark:border-gray-600"
          />
        </div>

        <div>
          <label class="block text-gray-600 dark:text-gray-300 text-sm font-medium mb-1">Password</label>
          <input
            v-model="password"
            type="password"
            placeholder="Enter password"
            class="w-full px-4 py-3 rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-purple-400 dark:bg-gray-700 dark:text-white dark:border-gray-600"
          />
        </div>

        <button
          @click="login"
          :disabled="loading"
          class="w-full bg-purple-600 text-white py-3 rounded-xl hover:bg-purple-700 transition font-semibold text-lg disabled:opacity-50">
          {{ loading ? 'Logging in...' : 'Login' }}
        </button>

        <!-- Hint -->
        <p class="text-center text-gray-400 text-sm">
          Test: username: <span class="text-purple-500 font-medium">emilys</span> / password: <span class="text-purple-500 font-medium">emilyspass</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  close: []
  loggedIn: [name: string, token: string]
}>()

const username = ref('')
const password = ref('')
const loading = ref(false)
const error = ref('')

const login = async () => {
  if (!username.value || !password.value) {
    error.value = 'Please enter username and password!'
    return
  }

  loading.value = true
  error.value = ''

  try {
    const res = await fetch('https://dummyjson.com/user/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: username.value,
        password: password.value,
        expiresInMins: 30
      })
    })

    const data = await res.json()

    if (data.accessToken) {
  localStorage.setItem('token', data.accessToken)
  localStorage.setItem('user', 'Mahi Yapa')
  emit('loggedIn', 'Mahi Yapa', data.accessToken)
    } else {
      error.value = 'Invalid username or password!'
    }
  } catch {
    error.value = 'Something went wrong. Try again!'
  } finally {
    loading.value = false
  }
}
</script>