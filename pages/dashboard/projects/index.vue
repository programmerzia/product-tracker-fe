<script setup lang="ts">
import { Plus, Pencil } from 'lucide-vue-next'
import { useAuthStore } from '~/stores/auth'
import { createError } from 'h3'
import { useFetchProjects } from '~/composables/useFetchProjects'
import { useFetchProducts } from '~/composables/useFetchProducts'
import ProjectFormDialog from './ProjectFormDialog.vue'

definePageMeta({
  middleware: 'auth',
  layout: 'dashboard'
})

const auth = useAuthStore()

onMounted(() => {
  if (!auth.canManageProjects) {
    throw createError({
      statusCode: 403,
      statusMessage: 'Access denied. Product Manager or Project Manager role required.'
    })
  }
})

const { projects, fetchProjects } = useFetchProjects()
const { products } = useFetchProducts()

const dialogOpen = ref(false)
const editingProject = ref(null)

const openNew = () => {
  editingProject.value = null
  dialogOpen.value = true
}

const openEdit = (project: any) => {
  editingProject.value = project
  dialogOpen.value = true
}
</script>

<template>
  <div class="max-w-4xl mx-auto my-8 p-4">
    <header class="flex justify-between items-center mb-6">
      <div>
        <h1 class="text-2xl font-semibold text-foreground">Projects</h1>
        <p class="text-muted-foreground text-sm mt-1">Manage production projects and track progress.</p>
      </div>
      <button @click="openNew" class="bg-primary text-primary-foreground px-4 py-2 rounded hover:bg-primary/90 font-medium flex items-center">
        <Plus class="h-4 w-4 mr-2" />
        Add Project
      </button>
    </header>

    <section class="rounded p-4 bg-card border border-border">
      <table class="w-full text-left">
        <thead>
          <tr class="border-b text-muted-foreground text-sm">
            <th class="py-2">Project</th>
            <th class="py-2">Product</th>
            <th class="py-2">Description</th>
            <th class="py-2">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="project in projects"
            :key="project.id"
            class="border-b"
          >
            <td class="py-2 text-foreground">{{ project.name }}</td>
            <td class="py-2 text-foreground">{{ project.product?.name }}</td>
            <td class="py-2 text-foreground">{{ project.description }}</td>
            <td class="py-2">
              <button @click="openEdit(project)" class="text-primary hover:underline flex items-center gap-1">
                <Pencil class="w-4 h-4" /> Edit
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>

    <ProjectFormDialog
      v-model:open="dialogOpen"
      :project="editingProject"
      :products="products ?? []"
      @submitted="fetchProjects"
    />
  </div>
</template>
