<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="80" width="80" :src="imageUrl" alt="info Imasge" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="mt-1 text-gray-400">{{ descr }}</p>
  </div>
</template>

<script setup>
defineProps({
  imageUrl: String,
  title: String,
  descr: String
})
</script>

<style lang="scss" scoped></style>
