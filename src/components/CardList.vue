<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :img-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :is-added="item.isAdded"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorite', item)"
      :on-click-add="() => emit('addToCart', item)"
    />
  </div>
</template>
<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  addToCart: Function
})
const emit = defineEmits(['addToFavorite'])
</script>
