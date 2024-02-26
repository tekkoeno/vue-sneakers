<template>
  <Drawer
    v-auto-animate
    v-if="drawerOpen && drawerOpen === true"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    :order-open="orderOpen"
    @open-order="openOrder"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" :vat-price="vatPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
<script setup>
import { ref, watch, provide, computed } from 'vue'

import axios from 'axios'

import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'

/* Корзина START*/
const cart = ref([])

const drawerOpen = ref(false)
const orderOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((obj, sum) => obj + sum.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value / 100) * 5))

const closeDrawer = () => {
  drawerOpen.value = false
  document.body.style.overflow = 'visible'
}
const openDrawer = () => {
  drawerOpen.value = true
  document.body.style.overflow = 'hidden'
}
const openOrder = () => {
  orderOpen.value = true
}
const closeOrder = () => {
  orderOpen.value = false
}
const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)
provide('cart', {
  closeDrawer,
  closeOrder,
  cart,
  addToCart,
  removeFromCart
})

/* Корзина END */
</script>
<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  padding: 0;
}
</style>
