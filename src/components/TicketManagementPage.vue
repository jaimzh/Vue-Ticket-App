<template>
  <div class="min-h-screen bg-gray-50">
    <Navbar
      :current-page="currentPage"
      @navigate="$emit('navigate', $event)"
      @logout="$emit('logout')"
    />

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div class="mb-8">
        <h1 class="text-4xl mb-2">Ticket Management</h1>
        <p class="text-gray-600">Create and manage your support tickets</p>
      </div>

      <!-- Create Ticket -->
      <div class="shadow-md mb-8 border rounded">
        <div class="p-4 border-b">
          <h3 class="text-base font-medium">Create New Ticket</h3>
          <p class="text-sm text-gray-500">
            Fill in the details to create a new ticket
          </p>
        </div>
        <div class="p-4">
          <form @submit.prevent="handleSubmit" class="space-y-4">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div class="space-y-2">
                <label for="title"
                  >Title <span class="text-red-500">*</span></label
                >
                <input
                  id="title"
                  placeholder="Enter ticket title"
                  v-model="title"
                  :class="[
                    'w-full border rounded px-3 py-2',
                    errors.title && 'border-red-500',
                  ]"
                />
                <p v-if="errors.title" class="text-sm text-red-500">
                  {{ errors.title }}
                </p>
              </div>

              <div class="space-y-2">
                <label for="priority">Priority</label>
                <select
                  id="priority"
                  v-model="priority"
                  class="w-full border rounded px-3 py-2 bg-white"
                >
                  <option value="low">Low</option>
                  <option value="medium">Medium</option>
                  <option value="high">High</option>
                </select>
              </div>
            </div>

            <div class="space-y-2">
              <label for="status">Status</label>
              <select
                id="status"
                v-model="status"
                class="w-full border rounded px-3 py-2 bg-white"
              >
                <option value="open">Open</option>
                <option value="in_progress">In Progress</option>
                <option value="closed">Closed</option>
              </select>
            </div>

            <div class="space-y-2">
              <label for="description">Description</label>
              <textarea
                id="description"
                rows="4"
                v-model="description"
                placeholder="Enter ticket description (optional)"
                class="w-full border rounded px-3 py-2"
              ></textarea>
            </div>

            <button
              type="submit"
              class="bg-blue-600 hover:bg-blue-700 text-white rounded px-4 py-2"
            >
              Add Ticket
            </button>
          </form>
        </div>
      </div>

      <!-- Tickets List -->
      <div>
        <h2 class="text-2xl mb-4">All Tickets ({{ tickets.length }})</h2>

        <div v-if="tickets.length === 0" class="shadow-md border rounded">
          <div class="py-12 text-center text-gray-500">
            No tickets yet. Create your first ticket above!
          </div>
        </div>

        <div
          v-else
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
        >
          <div
            v-for="t in tickets"
            :key="t.id"
            class="shadow-md hover:shadow-lg transition-shadow duration-300 border rounded"
          >
            <div class="p-4 border-b flex justify-between items-start gap-2">
              <h4 class="text-lg">{{ t.title }}</h4>
              <div class="flex gap-2">
                <button
                  class="h-8 w-8 rounded hover:bg-gray-100"
                  @click="handleEdit(t)"
                >
                  <Pencil class="h-4 w-4 text-blue-600" />
                </button>
                <button
                  class="h-8 w-8 rounded hover:bg-gray-100"
                  @click="openDelete(t.id)"
                >
                  <Trash2 class="h-4 w-4 text-red-600" />
                </button>
              </div>
            </div>
            <div class="p-4 space-y-3">
              <div class="flex gap-2 md:gap-4">
                <span
                  class="px-2 py-1 text-sm rounded border"
                  :class="statusBadge(t.status)"
                >
                  {{ t.status.replace("_", " ") }}
                </span>
                <span
                  class="px-2 py-1 text-sm rounded border"
                  :class="priorityBadge(t.priority)"
                >
                  {{ t.priority }}
                </span>
              </div>
              <p
                v-if="t.description"
                class="text-sm text-gray-600 line-clamp-3"
              >
                {{ t.description }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Edit Modal -->
    <div
      v-if="editDialogOpen"
      class="fixed inset-0 z-50 flex items-center justify-center"
    >
      <div
        class="absolute inset-0 bg-black/30"
        @click="editDialogOpen = false"
      ></div>
      <div class="relative z-10 bg-white border rounded w-full max-w-lg p-6">
        <h3 class="text-lg font-medium mb-1">Edit Ticket</h3>
        <p class="text-sm text-gray-500 mb-4">Make changes to your ticket</p>

        <div v-if="editingTicket" class="space-y-4">
          <div class="space-y-2">
            <label for="edit-title"
              >Title <span class="text-red-500">*</span></label
            >
            <input
              id="edit-title"
              v-model="editingTicket.title"
              class="w-full border rounded px-3 py-2"
            />
          </div>

          <div class="space-y-2">
            <label for="edit-status">Status</label>
            <select
              id="edit-status"
              v-model="editingTicket.status"
              class="w-full border rounded px-3 py-2 bg-white"
            >
              <option value="open">Open</option>
              <option value="in_progress">In Progress</option>
              <option value="closed">Closed</option>
            </select>
          </div>

          <div class="space-y-2">
            <label for="edit-priority">Priority</label>
            <select
              id="edit-priority"
              v-model="editingTicket.priority"
              class="w-full border rounded px-3 py-2 bg-white"
            >
              <option value="low">Low</option>
              <option value="medium">Medium</option>
              <option value="high">High</option>
            </select>
          </div>

          <div class="space-y-2">
            <label for="edit-description">Description</label>
            <textarea
              id="edit-description"
              rows="4"
              v-model="editingTicket.description"
              class="w-full border rounded px-3 py-2"
            />
          </div>
        </div>

        <div class="mt-6 flex justify-end gap-2">
          <button
            class="border rounded px-3 py-2"
            @click="editDialogOpen = false"
          >
            Cancel
          </button>
          <button
            class="bg-blue-600 hover:bg-blue-700 text-white rounded px-3 py-2"
            @click="saveEdit"
          >
            Save Changes
          </button>
        </div>
      </div>
    </div>

    <!-- Delete Confirm Modal -->
    <div
      v-if="deleteDialogOpen"
      class="fixed inset-0 z-50 flex items-center justify-center"
    >
      <div
        class="absolute inset-0 bg-black/30"
        @click="deleteDialogOpen = false"
      ></div>
      <div class="relative z-10 bg-white border rounded w-full max-w-md p-6">
        <h3 class="text-lg font-medium">Are you sure?</h3>
        <p class="text-sm text-gray-500 mt-1">
          This action cannot be undone. This will permanently delete the ticket.
        </p>
        <div class="mt-6 flex justify-end gap-2">
          <button
            class="border rounded px-3 py-2"
            @click="deleteDialogOpen = false"
          >
            Cancel
          </button>
          <button
            class="bg-red-600 hover:bg-red-700 text-white rounded px-3 py-2"
            @click="confirmDelete"
          >
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Navbar from "./Navbar.vue";
import { Pencil, Trash2 } from "lucide-vue-next";
import { ref } from "vue";
import { toast } from "vue-sonner";

interface Ticket {
  id: string;
  title: string;
  description: string;
  status: "open" | "in_progress" | "closed";
  priority: "low" | "medium" | "high";
}

const props = defineProps<{
  currentPage: string;
  tickets: Ticket[];
  onAddTicket: (t: Omit<Ticket, "id">) => void;
  onUpdateTicket: (id: string, t: Omit<Ticket, "id">) => void;
  onDeleteTicket: (id: string) => void;
}>();

defineEmits<{ (e: "navigate", page: string): void; (e: "logout"): void }>();

const title = ref("");
const description = ref("");
const status = ref<"open" | "in_progress" | "closed">("open");
const priority = ref<"low" | "medium" | "high">("medium");
const errors = ref<{ title: string }>({ title: "" });

const editingTicket = ref<Ticket | null>(null);
const editDialogOpen = ref(false);
const deleteDialogOpen = ref(false);
const ticketToDelete = ref<string | null>(null);

function resetForm() {
  title.value = "";
  description.value = "";
  status.value = "open";
  priority.value = "medium";
  errors.value = { title: "" };
}

function handleSubmit() {
  if (!title.value.trim()) {
    errors.value.title = "Title is required";
    toast.error("Please enter a ticket title");
    return;
  }
  props.onAddTicket({
    title: title.value.trim(),
    description: description.value.trim(),
    status: status.value,
    priority: priority.value,
  });
  toast.success("Ticket created successfully!");
  resetForm();
}

function handleEdit(t: Ticket) {
  editingTicket.value = { ...t };
  editDialogOpen.value = true;
}

function saveEdit() {
  if (!editingTicket.value) return;
  if (!editingTicket.value.title.trim()) {
    toast.error("Title is required");
    return;
  }
  props.onUpdateTicket(editingTicket.value.id, {
    title: editingTicket.value.title,
    description: editingTicket.value.description,
    status: editingTicket.value.status,
    priority: editingTicket.value.priority,
  });
  toast.success("Ticket updated successfully!");
  editDialogOpen.value = false;
  editingTicket.value = null;
}

function openDelete(id: string) {
  ticketToDelete.value = id;
  deleteDialogOpen.value = true;
}

function confirmDelete() {
  if (ticketToDelete.value) {
    props.onDeleteTicket(ticketToDelete.value);
    toast.success("Ticket deleted successfully!");
    deleteDialogOpen.value = false;
    ticketToDelete.value = null;
  }
}

function statusBadge(s: string) {
  if (s === "open") return "bg-green-100 text-green-700 border-green-200";
  if (s === "in_progress")
    return "bg-amber-100 text-amber-700 border-amber-200";
  return "bg-gray-100 text-gray-700 border-gray-200";
}

function priorityBadge(p: string) {
  if (p === "high") return "bg-red-100 text-red-700 border-red-200";
  if (p === "medium") return "bg-yellow-100 text-yellow-700 border-yellow-200";
  if (p === "low") return "bg-blue-100 text-blue-700 border-blue-200";
  return "bg-gray-100 text-gray-700";
}
</script>
