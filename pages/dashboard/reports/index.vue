<script setup lang="ts">
import { onMounted } from 'vue'
import { useAuthStore } from '~/stores/auth'
import { createError } from 'h3'

definePageMeta({
  middleware: 'auth',
  layout: 'dashboard'  // <-- assign layout here
})

const { canViewSummaries } = useAuthStore()

onMounted(() => {
  if (!canViewSummaries) {
    throw createError({
      statusCode: 403,
      statusMessage: 'Access denied. Project Manager role required.'
    })
  }
})
</script>

<template>
  <div class="max-w-3xl mx-auto my-8 p-4 space-y-6">
    <header class="sm:flex sm:items-center sm:justify-between">
      <div class="sm:flex-auto">
        <h1 class="text-2xl font-semibold text-gray-900">Reports & Analytics</h1>
        <p class="mt-2 text-sm text-gray-700">View project summaries and performance analytics.</p>
      </div>
    </header>

    <section class="bg-white shadow rounded-lg p-6 text-center text-gray-500">
      Reports and analytics interface will be implemented here.<br />
      This page is accessible to Project Managers only.
    </section>
  </div>
</template>
