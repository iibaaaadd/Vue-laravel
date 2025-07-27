<template>
  <div class="p-4">
    <h1 class="text-xl font-bold mb-4">CRUD Produk</h1>
    <form @submit.prevent="saveProduct" class="mb-4">
      <input v-model="form.name" placeholder="Nama Produk" class="border p-2 mr-2" />
      <input v-model="form.price" type="number" placeholder="Harga" class="border p-2 mr-2" />
      <button class="bg-blue-500 text-white px-4 py-2">Simpan</button>
    </form>

    <ul>
      <li v-for="item in products" :key="item.id" class="mb-2">
        {{ item.name }} - Rp{{ item.price }}
        <button @click="editProduct(item)" class="ml-2 text-yellow-500">Edit</button>
        <button @click="deleteProduct(item.id)" class="ml-2 text-red-500">Hapus</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const products = ref([])
const form = ref({ id: null, name: '', price: '' })

const fetchProducts = async () => {
  const res = await axios.get('/api/products')
  products.value = res.data
}

const saveProduct = async () => {
  if (form.value.id) {
    await axios.put(`/api/products/${form.value.id}`, form.value)
  } else {
    await axios.post('/api/products', form.value)
  }
  form.value = { id: null, name: '', price: '' }
  fetchProducts()
}

const editProduct = (item) => {
  form.value = { ...item }
}

const deleteProduct = async (id) => {
  await axios.delete(`/api/products/${id}`)
  fetchProducts()
}

onMounted(fetchProducts)
</script>
