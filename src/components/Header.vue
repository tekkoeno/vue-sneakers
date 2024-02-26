<template>
  <header class="flex justify-between border-b px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10 items-center" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex item-center gap-3 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="cart" />
        <b>{{ totalPrice - vatPrice }} 원</b>
      </li>
      <router-link to="/favorites">
        <li class="flex item-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="heart" />
          <span>Закладки</span>
        </li>
      </router-link>

      <router-link to="/orders">
        <li class="flex item-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/profile.svg" alt="profile" />
          <span>Профиль</span>
        </li>
      </router-link>
    </ul>
  </header>
</template>
<script setup>
defineProps({
  openDrawer: Function,
  totalPrice: Number,
  vatPrice: Number
})
const emit = defineEmits(['openDrawer'])
</script>
