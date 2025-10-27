<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 via-white to-purple-50 flex items-center justify-center p-4 relative">
    <div class="absolute top-10 right-20 w-72 h-72 bg-blue-200 rounded-full opacity-20 blur-3xl"></div>
    <div class="absolute bottom-20 left-10 w-96 h-96 bg-purple-200 rounded-full opacity-20 blur-3xl"></div>

    <div class="w-full max-w-md shadow-xl relative z-10 border rounded bg-white">
      <div class="p-6 border-b space-y-1 text-center">
        <h2 class="text-3xl">Welcome Back</h2>
        <p class="text-gray-600">Sign in to your TicketFlow account</p>
      </div>

      <div class="p-6">
        <form @submit.prevent="handleSubmit" class="space-y-4">
          <!-- Email -->
          <div class="space-y-2">
            <label for="email">Email</label>
            <div class="relative">
              <input
                id="email"
                type="email"
                placeholder="you@example.com"
                v-model.trim="email"
                :aria-invalid="emailTouched && !emailValid"
                :class="[
                  'w-full border rounded px-3 py-2 pr-9 transition-colors',
                  emailTouched && !emailValid ? 'border-red-500' : '',
                  emailTouched && emailValid ? 'border-green-500' : ''
                ]"
                @blur="emailTouched = true"
              />
              <!-- status dot -->
              <span
                v-if="emailTouched"
                class="absolute right-3 top-1/2 -translate-y-1/2 h-2.5 w-2.5 rounded-full"
                :class="emailValid ? 'bg-green-500' : 'bg-red-500'"
              />
            </div>
            <p v-if="emailTouched && !emailValid" class="text-sm text-red-500">Please enter a valid email</p>
            <p v-else-if="emailTouched && emailValid" class="text-xs text-green-600">Looks good.</p>
          </div>

          <!-- Password -->
          <div class="space-y-2">
            <label for="password">Password</label>
            <div class="relative">
              <input
                id="password"
                type="password"
                placeholder="••••••••"
                v-model="password"
                :aria-invalid="passwordTouched && !passwordValid"
                :class="[
                  'w-full border rounded px-3 py-2 pr-9 transition-colors',
                  passwordTouched && !passwordValid ? 'border-red-500' : '',
                  passwordTouched && passwordValid ? 'border-green-500' : ''
                ]"
                @blur="passwordTouched = true"
              />
              <span
                v-if="passwordTouched"
                class="absolute right-3 top-1/2 -translate-y-1/2 h-2.5 w-2.5 rounded-full"
                :class="passwordValid ? 'bg-green-500' : 'bg-red-500'"
              />
            </div>
            <p class="text-xs text-gray-500">Minimum 6 characters.</p>
            <p v-if="passwordTouched && !passwordValid" class="text-sm text-red-500">Password must be at least 6 characters</p>
            <p v-else-if="passwordTouched && passwordValid" class="text-xs text-green-600">Strong enough to pass this very low bar.</p>
          </div>

          <div class="flex items-center justify-end">
            <a href="#" class="text-sm text-blue-600 hover:underline">Forgot password?</a>
          </div>

          <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white rounded px-4 py-2">
            Sign In
          </button>
        </form>
      </div>

      <div class="p-6 border-t text-center">
        <p class="text-sm text-gray-600">
          Don't have an account?
          <button class="text-blue-600 hover:underline" @click="$emit('navigate', 'signup')">Sign up</button>
        </p>
      </div>

      <!-- Back to Landing -->
      <div class="mt-6 text-center relative z-10">
        <button class="text-sm text-blue-600 hover:underline" @click="$emit('navigate', 'landing')">← Back to Home</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { toast } from 'vue-sonner'

const emit = defineEmits<{ (e: 'navigate', page: string): void; (e: 'login'): void }>()

const email = ref('')
const password = ref('')

const emailTouched = ref(false)
const passwordTouched = ref(false)

const emailValid = computed(() => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value))
const passwordValid = computed(() => (password.value?.length || 0) >= 6)

function handleSubmit() {
  emailTouched.value = true
  passwordTouched.value = true

  if (emailValid.value && passwordValid.value) {
    toast.success('Login successful!')
    setTimeout(() => { emit('login'); emit('navigate', 'dashboard') }, 500)
  } else {
    toast.error('Please fix the errors in the form')
  }
}
</script>
