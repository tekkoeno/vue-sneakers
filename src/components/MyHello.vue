<template>
  <div class="item">
    <h2 class="title">{{ title }}</h2>
    <p class="price">{{ price }} 원</p>
    <p>Добавлен: {{ isAdded }}</p>
  </div>
</template>
<script setup>
defineProps({
  title: String,
  price: Number,
  isAdded: Boolean
})
</script>
<style scoped>
.item {
  border: 1px solid royalblue;
  padding: 10px;
  margin-bottom: 10px;
}
.price {
  font-size: 30px;
}
</style>
