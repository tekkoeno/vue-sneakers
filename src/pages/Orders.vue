<template>
  <h2 class="text-3xl font-bold mb-10">Мои закладки</h2>
  <div class="grid grid-cols-4 gap-4" v-auto-animate>
    <div v-for="order in orders" :key="order.id">
      <div v-for="item in order.items" :key="item.id">
        <Card
          :img-url="item.imageUrl"
          :title="item.title"
          :price="item.price"
          :is-added="item.isAdded"
          :is-favorite="item.isFavorite"
          class="h-full"
        />
      </div>
    </div>
  </div>
</template>
<script setup>
import OrderItem from '../components/OrderItem.vue'
import { ref, onMounted } from 'vue'
import axios from 'axios'
import Card from '@/components/Card.vue'
const orders = ref([])
onMounted(async () => {
  try {
    const { data } = await axios.get('https://6fbd4cce9fa1e626.mokky.dev/orders')
    orders.value = data.map((item) => item)
  } catch (error) {
    alert(error)
  }
})
</script>
<style scoped></style>
