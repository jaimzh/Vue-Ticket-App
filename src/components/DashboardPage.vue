<template>
  <div class="min-h-screen bg-gray-50">
    <Navbar
      :current-page="currentPage"
      @navigate="$emit('navigate', $event)"
      @logout="$emit('logout')"
    />

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div class="mb-8">
        <h1 class="text-4xl mb-2">Dashboard</h1>
        <p class="text-gray-600">Welcome back! Here's an overview of your tickets.</p>
      </div>

      <!-- Stats Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <div
          v-for="(s, i) in stats"
          :key="i"
          class="shadow-md hover:shadow-lg transition-shadow duration-300 border rounded"
        >
          <div class="flex flex-row items-center justify-between p-4 pb-2">
            <h4 class="text-sm">{{ s.title }}</h4>
            <div class="p-2 rounded-lg" :class="s.bgColor">
              <component :is="s.icon" class="w-8 h-8" :class="s.iconColor" />
            </div>
          </div>
          <div class="p-4 pt-0">
            <div class="text-3xl" :class="s.textColor">
              {{ s.value }}
            </div>
          </div>
        </div>
      </div>

      <!-- Recent Activity -->
      <div class="shadow-md mt-6 border rounded">
        <div class="p-4 border-b">
          <h3 class="text-base font-medium">Recent Activity</h3>
          <p class="text-sm text-gray-500">Latest updates on your tickets</p>
        </div>
        <div class="p-4">
          <p v-if="tickets.length === 0" class="text-gray-500 text-center py-8">
            No tickets yet. Create your first ticket to get started!
          </p>
          <div v-else class="space-y-4">
            <div
              v-for="t in tickets.slice(0, 5)"
              :key="t.id"
              class="flex items-center justify-between py-3 border-b last:border-b-0"
            >
              <div>
                <p class="text-gray-900">{{ t.title }}</p>
                <p class="text-sm text-gray-500">Priority: {{ t.priority }}</p>
              </div>
              <span
                class="px-3 py-1 rounded-full text-sm"
                :class="statusClass(t.status)"
              >
                {{ t.status.replace('_', ' ') }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import Navbar from './Navbar.vue'
import { computed } from 'vue'
import { Ticket as TicketIcon, Clock, CheckCircle2, XCircle } from 'lucide-vue-next'

interface Ticket {
  id: string
  title: string
  description: string
  status: 'open' | 'in_progress' | 'closed'
  priority: 'low' | 'medium' | 'high'
}

const props = defineProps<{ currentPage: string; tickets: Ticket[] }>()
defineEmits<{ (e: 'navigate', page: string): void; (e: 'logout'): void }>()

const total = computed(() => props.tickets.length)
const openCount = computed(() => props.tickets.filter(t => t.status === 'open').length)
const inProg = computed(() => props.tickets.filter(t => t.status === 'in_progress').length)
const closed = computed(() => props.tickets.filter(t => t.status === 'closed').length)

function statusClass(status: string) {
  if (status === 'open') return 'bg-green-100 text-green-700'
  if (status === 'in_progress') return 'bg-amber-100 text-amber-700'
  return 'bg-gray-100 text-gray-700'
}

const stats = computed(() => [
  { title: 'Total Tickets',   value: total.value,   icon: TicketIcon,  iconColor: 'text-blue-500',  bgColor: 'bg-blue-50',  textColor: 'text-blue-600' },
  { title: 'Open Tickets',    value: openCount.value, icon: XCircle,     iconColor: 'text-green-500', bgColor: 'bg-green-50', textColor: 'text-green-600' },
  { title: 'In Progress',     value: inProg.value,  icon: Clock,       iconColor: 'text-amber-500', bgColor: 'bg-amber-50', textColor: 'text-amber-600' },
  { title: 'Closed Tickets',  value: closed.value,  icon: CheckCircle2, iconColor: 'text-gray-500',  bgColor: 'bg-gray-50', textColor: 'text-gray-600' },
])
</script>
