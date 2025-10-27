<template>
  <div class="max-w-[1440px] mx-auto">
    <LandingPage v-if="currentPage === 'landing'" @navigate="handleNavigate" />

    <LoginPage v-else-if="currentPage === 'login'" @navigate="handleNavigate" @login="handleLogin" />

    <SignupPage v-else-if="currentPage === 'signup'" @navigate="handleNavigate" @login="handleLogin" />

    <DashboardPage
      v-else-if="currentPage === 'dashboard' && isAuthenticated"
      :current-page="currentPage"
      :tickets="tickets"
      @navigate="handleNavigate"
      @logout="handleLogout"
    />

    <TicketManagementPage
      v-else-if="currentPage === 'tickets' && isAuthenticated"
      :current-page="currentPage"
      :tickets="tickets"
      :on-add-ticket="handleAddTicket"
      :on-update-ticket="handleUpdateTicket"
      :on-delete-ticket="handleDeleteTicket"
      @navigate="handleNavigate"
      @logout="handleLogout"
    />

    <div v-else class="min-h-screen flex items-center justify-center p-10 text-center">
      <div>
        <h1 class="text-3xl mb-2">{{ currentPage }}</h1>
        <p class="text-gray-600">We’ll wire this page next.</p>
        <button class="mt-6 px-4 py-2 border rounded" @click="handleNavigate('landing')">Back to Landing</button>
      </div>
    </div>

    <Toaster />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { Toaster } from 'vue-sonner'
import LandingPage from './components/LandingPage.vue'
import LoginPage from './components/LoginPage.vue'
import SignupPage from './components/SignupPage.vue'
import DashboardPage from './components/DashboardPage.vue'
import TicketManagementPage from './components/TicketManagementPage.vue'

export interface Ticket {
  id: string
  title: string
  description: string
  status: 'open' | 'in_progress' | 'closed'
  priority: 'low' | 'medium' | 'high'
}

const currentPage = ref<string>('landing')
const isAuthenticated = ref(false)
const tickets = ref<Ticket[]>([])

function initTickets() {
  const saved = localStorage.getItem('tickets')
  if (saved) {
    try { tickets.value = JSON.parse(saved) } catch { tickets.value = [] }
  } else {
    tickets.value = [
      { id: '1', title: 'Fix login bug', description: 'Users are unable to login with their credentials', status: 'open', priority: 'high' },
      { id: '2', title: 'Update dashboard UI', description: 'Refresh the dashboard with new design system', status: 'in_progress', priority: 'medium' },
      { id: '3', title: 'Add export feature', description: 'Allow users to export reports as PDF', status: 'closed', priority: 'low' },
    ]
  }
}

onMounted(() => {
  const savedAuth = localStorage.getItem('isAuthenticated')
  const savedPage = localStorage.getItem('currentPage')
  if (savedAuth === 'true') {
    isAuthenticated.value = true
    currentPage.value = savedPage || 'dashboard'
  }
  initTickets()
})

watch(tickets, (val) => {
  localStorage.setItem('tickets', JSON.stringify(val))
}, { deep: true })

function handleNavigate(page: string) {
  currentPage.value = page
  localStorage.setItem('currentPage', page)
}

function handleLogin() {
  isAuthenticated.value = true
  localStorage.setItem('isAuthenticated', 'true')
  localStorage.setItem('currentPage', 'dashboard')
  currentPage.value = 'dashboard'
}

function handleLogout() {
  isAuthenticated.value = false
  localStorage.removeItem('isAuthenticated')
  localStorage.setItem('currentPage', 'login')
  currentPage.value = 'login'
}

function handleAddTicket(t: Omit<Ticket, 'id'>) {
  const newTicket: Ticket = { ...t, id: Date.now().toString() }
  tickets.value = [newTicket, ...tickets.value]
}

function handleUpdateTicket(id: string, t: Omit<Ticket, 'id'>) {
  tickets.value = tickets.value.map(ticket => ticket.id === id ? { ...t, id } : ticket)
}

function handleDeleteTicket(id: string) {
  tickets.value = tickets.value.filter(ticket => ticket.id !== id)
}
</script>
