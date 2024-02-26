<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <div>
      <img
        v-if="onClickFavorite"
        @click="onClickFavorite"
        :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
        alt="like-1"
        class="absolute top-8 left-8"
      />
      <img :src="imgUrl" alt="Sneaker" />
      <p class="mt-2">{{ title }}</p>
      <div class="flex justify-between">
        <div class="flex flex-col">
          <span class="text-slate-400">Price:</span>
          <b>{{ price }} 원</b>
        </div>
        <img
          v-if="onClickAdd"
          @click="onClickAdd"
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          alt="btn-blus"
        />
      </div>
    </div>
  </div>
</template>
<script setup>
defineProps({
  id: Number,
  imgUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>
