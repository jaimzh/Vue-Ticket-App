<template>
  <div class="min-h-screen bg-gradient-to-b from-blue-50 to-white">
    <!-- Hero -->
    <section class="relative overflow-hidden">
      <!-- Wavy background -->
      <div class="absolute inset-0 z-0">
        <svg class="absolute bottom-0 w-full h-64 md:h-96" viewBox="0 0 1440 320" preserveAspectRatio="none">
          <path
            fill="#3b82f6"
            fill-opacity="0.1"
            d="M0,96L48,112C96,128,192,160,288,165.3C384,171,480,149,576,128C672,107,768,85,864,90.7C960,96,1056,128,1152,144C1248,160,1344,160,1392,160L1440,160L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"
          />
        </svg>
        <svg class="absolute bottom-0 w-full h-64 md:h-96" viewBox="0 0 1440 320" preserveAspectRatio="none">
          <path
            fill="#3b82f6"
            fill-opacity="0.05"
            d="M0,224L48,213.3C96,203,192,181,288,181.3C384,181,480,203,576,224C672,245,768,267,864,261.3C960,256,1056,224,1152,208C1248,192,1344,192,1392,192L1440,192L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"
          />
        </svg>
      </div>

      <!-- Decorative blobs -->
      <div class="absolute top-20 right-10 w-64 h-64 bg-blue-200 rounded-full opacity-20 blur-3xl"></div>
      <div class="absolute top-40 left-20 w-96 h-96 bg-purple-200 rounded-full opacity-20 blur-3xl"></div>

      <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pt-20 pb-32">
        <div class="text-center">
          <h1 class="text-5xl md:text-7xl mb-6 bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
            TicketFlow
          </h1>
          <p class="text-xl md:text-2xl text-gray-600 mb-12 max-w-3xl mx-auto">
            Streamline your support workflow with intelligent ticket management
          </p>
          <div class="flex flex-col sm:flex-row gap-4 justify-center">
            <button
              class="px-5 py-3 bg-blue-600 hover:bg-blue-700 text-white rounded"
              @click="$emit('navigate', 'signup')"
            >
              Get Started
            </button>
            <button
              class="px-5 py-3 border rounded hover:bg-gray-50"
              @click="$emit('navigate', 'login')"
            >
              Login
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Features -->
    <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <div
          v-for="(feature, i) in features"
          :key="i"
          class="shadow-lg hover:shadow-xl transition-shadow duration-300  rounded p-6 bg-white"
        >
          <div class="mb-4">
            <component :is="feature.icon" class="w-12 h-12" :class="feature.color" />
          </div>
          <h3 class="text-lg font-medium">{{ feature.title }}</h3>
          <p class="text-gray-600">{{ feature.description }}</p>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-50 border-t border-gray-200 mt-20">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div class="flex flex-col md:flex-row justify-between items-center gap-4">
          <p class="text-gray-600">© 2025 TicketFlow. All rights reserved.</p>
          <div class="flex gap-6">
            <a href="#" class="text-gray-600 hover:text-blue-600 transition-colors">Privacy Policy</a>
            <a href="#" class="text-gray-600 hover:text-blue-600 transition-colors">Terms of Service</a>
            <a href="#" class="text-gray-600 hover:text-blue-600 transition-colors">Contact Us</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { CheckCircle2, Users, TrendingUp, Shield } from 'lucide-vue-next'

const features = [
  { icon: CheckCircle2, color: 'text-green-500', title: 'Manage Tickets', description: 'Create, organize, and track all your support tickets in one centralized place.' },
  { icon: TrendingUp, color: 'text-amber-500', title: 'Track Progress', description: 'Monitor ticket status in real-time with intuitive dashboards and analytics.' },
  { icon: Users, color: 'text-blue-500', title: 'Collaborate Easily', description: 'Work together seamlessly with your team to resolve issues faster.' },
  { icon: Shield, color: 'text-purple-500', title: 'Secure & Reliable', description: 'Your data is protected with enterprise-grade security and reliability.' },
]
</script>

<style scoped>
/* Tailwind handles visuals; no custom CSS needed here */
</style>
