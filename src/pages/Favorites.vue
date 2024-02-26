<template>
  <h2 class="text-3xl font-bold mb-10">Мои закладки</h2>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in favorites"
      :key="item.item.id"
      :img-url="item.item.imageUrl"
      :title="item.item.title"
      :price="item.item.price"
      :is-added="item.item.isAdded"
      :is-favorite="item.item.isFavorite"
    />
  </div>
</template>
<script setup>
import Card from '@/components/Card.vue'
import CartItem from '@/components/CartItem.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'
const favorites = ref([])
onMounted(async () => {
  try {
    const { data } = await axios.get('https://6fbd4cce9fa1e626.mokky.dev/favorites')
    favorites.value = data
  } catch (error) {
    alert(error)
  }
})
</script>
