<script setup lang="ts">
import { Plus } from 'lucide-vue-next'
import { onMounted } from 'vue'
import { useAuthStore } from '~/stores/auth'
import { createError } from 'h3'

definePageMeta({
  middleware: 'auth',
  layout: 'dashboard'  // this applies the dashboard layout automatically
})

const auth = useAuthStore()

onMounted(() => {
  if (!auth.canManageItems) {
    throw createError({
      statusCode: 403,
      statusMessage: 'Access denied. Production Engineer role required.'
    })
  }
})
</script>

<template>
  <div class="max-w-3xl mx-auto my-8 p-4 space-y-6">
    <header class="flex justify-between items-center">
      <div>
        <h1 class="text-2xl font-semibold text-gray-900">Product Items</h1>
        <p class="text-gray-600 mt-1 text-sm">Create and manage individual production items.</p>
      </div>
      <button
        type="button"
        class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 font-medium flex items-center"
      >
        <Plus class="h-4 w-4 mr-2" />
        Add Item
      </button>
    </header>

    <section class="bg-white border border-gray-200 rounded p-8 text-center text-gray-600">
      Product item management interface will be implemented here.<br />
      This page is accessible to Production Engineers only.
    </section>
  </div>
</template>
