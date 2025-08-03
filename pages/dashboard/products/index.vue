<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { Plus, Pencil } from 'lucide-vue-next'
import { useAuthStore } from '~/stores/auth'
import { createError } from 'h3'
import ProductFormDialog from './ProductFormDialog.vue'

definePageMeta({
  middleware: 'auth',
  layout: 'dashboard'
})

const auth = useAuthStore()

onMounted(() => {
  if (!auth.canManageProducts) {
    throw createError({
      statusCode: 403,
      statusMessage: 'Access denied. Product Manager role required.'
    })
  }
})

const { products, fetchProducts, error } = useFetchProducts()

const editingProduct = ref(null)
const dialogOpen = ref(false)

const handleEdit = (product: any) => {
  editingProduct.value = product
  dialogOpen.value = true
}
</script>

<template>
  <div class="max-w-4xl mx-auto my-8 p-4">
    <header class="flex justify-between items-center mb-6">
  <div>
    <h1 class="text-2xl font-semibold text-foreground">Products</h1>
    <p class="text-muted-foreground mt-1">Manage your product catalog and versions.</p>
  </div>
  <button
    @click="() => { editingProduct = null; dialogOpen = true }"
    
  >
    <!-- <Plus class="h-4 w-4 mr-2" />
    Add Product -->
    <ProductFormDialog
      v-model:open="dialogOpen"
      :product="editingProduct"
      @submitted="fetchProducts"
    />
  </button>
</header>

<section  class="rounded p-4 bg-card border border-border">
     <table class="w-full text-left">
  <thead>
    <tr class="border-b text-sm ">
      <th class="py-2">Name</th>
      <th class="py-2">Version</th>
      <th class="py-2">Description</th>
      <th class="py-2">Price</th>
      <th class="py-2">Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr
      v-for="product in products"
      :key="product.id"
      class="border-b "
    >
      <td class="py-2 text-foreground">{{ product.name }}</td>
      <td class="py-2 text-foreground">{{ product.version }}</td>
      <td class="py-2 text-foreground">{{ product.description }}</td>
      <td class="py-2 text-foreground">{{ product.pricing }}</td>
      <td class="py-2">
        <button
          @click="handleEdit(product)"
          class="text-primary hover:underline flex items-center gap-1"
        >
          <Pencil class="w-4 h-4" /> Edit
        </button>
      </td>
    </tr>
  </tbody>
</table>

    </section>
  </div>
</template>
