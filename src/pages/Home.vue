<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-10">Все кроссовки</h2>
    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        class="py-2 px-3 border rounded-md outline-none cursor-pointer"
      >
        <option value="title">By name</option>
        <option value="price">Price (cheapest)</option>
        <option value="-price">Price (expensive)</option>
      </select>
      <div class="relative">
        <img class="absolute left-3 top-3" src="/search.svg" alt="" />
        <input
          @input="onChangeSearchInput"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Search"
        />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList @add-to-favorite="addToFavorite" :items="items" @add-to-cart="onClickAddPlus" />
  </div>
</template>
<script setup>
import CardList from '../components/CardList.vue'
import axios from 'axios'
import { inject, onMounted, provide, reactive, ref, watch } from 'vue'
const { cart, addToCart, removeFromCart } = inject('cart')
const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://6fbd4cce9fa1e626.mokky.dev/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      favoriteId: null,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    alert(error)
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://6fbd4cce9fa1e626.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    alert(error)
  }
}
const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}
const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true
      const { data } = await axios.post('https://6fbd4cce9fa1e626.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://6fbd4cce9fa1e626.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    alert(error)
  }
}
const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []
  await fetchItems()
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
provide('items', items)
</script>
