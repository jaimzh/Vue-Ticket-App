<template>
  <header class="bg-white border-b border-gray-200 sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex justify-between items-center h-16">
        <!-- Logo -->
        <div class="cursor-pointer" @click="$emit('navigate', 'dashboard')">
          <span class="bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
            TicketFlow
          </span>
        </div>

        <!-- Desktop Navigation -->
        <nav class="hidden md:flex items-center gap-6">
          <button
            @click="$emit('navigate', 'dashboard')"
            :class="['hover:text-blue-600 transition-colors', currentPage === 'dashboard' ? 'text-blue-600' : 'text-gray-700']"
          >
            Dashboard
          </button>
          <button
            @click="$emit('navigate', 'tickets')"
            :class="['hover:text-blue-600 transition-colors', currentPage === 'tickets' ? 'text-blue-600' : 'text-gray-700']"
          >
            Tickets
          </button>
          <button class="ml-4 border rounded px-3 py-1" @click="handleLogout">
            Logout
          </button>
        </nav>

        <!-- Mobile Menu Button -->
        <button class="md:hidden p-2" @click="mobileMenuOpen = !mobileMenuOpen">
          <component :is="mobileMenuOpen ? X : Menu" class="w-6 h-6" />
        </button>
      </div>

      <!-- Mobile Menu -->
      <div v-if="mobileMenuOpen" class="md:hidden py-4 border-t border-gray-200">
        <nav class="flex flex-col gap-4">
          <button
            @click="navigateAndClose('dashboard')"
            :class="['text-left py-2 px-4 rounded hover:bg-gray-100', currentPage === 'dashboard' ? 'text-blue-600 bg-blue-50' : 'text-gray-700']"
          >
            Dashboard
          </button>
          <button
            @click="navigateAndClose('tickets')"
            :class="['text-left py-2 px-4 rounded hover:bg-gray-100', currentPage === 'tickets' ? 'text-blue-600 bg-blue-50' : 'text-gray-700']"
          >
            Tickets
          </button>
          <button class="mx-4 border rounded px-3 py-2" @click="handleLogout">
            Logout
          </button>
        </nav>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Menu, X } from 'lucide-vue-next'

defineProps<{ currentPage: string }>()
const emit = defineEmits<{ (e: 'navigate', page: string): void; (e: 'logout'): void }>()

const mobileMenuOpen = ref(false)

function navigateAndClose(page: string) {
  emit('navigate', page)
  mobileMenuOpen.value = false
}
function handleLogout() {
  emit('logout')
  emit('navigate', 'landing')
  mobileMenuOpen.value = false
}
</script>
