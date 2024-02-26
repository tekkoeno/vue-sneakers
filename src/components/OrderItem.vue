<template>
  <div v-for="order in orders" :key="order.id">
    <div v-for="item in order.items" :key="item.id">
      <Card
        :img-url="item.imageUrl"
        :title="item.title"
        :price="item.price"
        :is-added="item.isAdded"
        :is-favorite="item.isFavorite"
        class="h-[190px]"
      />
    </div>
  </div>
</template>
<script setup>
import Card from '@/components/Card.vue'
import axios from 'axios'
</script>
