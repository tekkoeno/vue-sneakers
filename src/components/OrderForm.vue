<template>
  <div
    class="absolute left-[30%] right-[50%] top-[15%] bg-white mh-[530px] w-[600px] z-30 rounded-xl p-10"
    v-auto-animate
  >
    <div class="flex flex-col justify-between">
      <svg
        @click="closeOrder"
        class="opacity-30 absolute top-[50px] cursor-pointer rotate-180 hover:opacity-100 transition hover:-translate-x-1"
        width="16"
        height="14"
        viewBox="0 0 16 14"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M1 7H14.7143"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M8.71436 1L14.7144 7L8.71436 13"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      <h3 class="text-3xl mb-10 font-bold text-center">Заказ</h3>

      <div class="flex flex-col gap-1 h-[176px] overflow-x-hidden">
        <div class="flex items-center justify-between" v-for="item in cart" :key="item.id">
          <img class="w-16 h-14" :src="item.imageUrl" alt="sneaker" />
          <h2 class="text-sm">{{ item.title }}</h2>
          <p>{{ item.price }} 원</p>
        </div>
      </div>
      <div class="flex flex-col mt-10 gap-4 w-80 justify-center text-center m-auto">
        <input
          v-model="isNameValue"
          class="input pl-3 p-[5px] pr-3"
          type="text"
          placeholder="Your name"
        />
        <input
          v-model="isNumberValue"
          @input="formatPhoneNumber"
          class="input pl-3 p-[5px] pr-3"
          type="text"
          placeholder="(___) __-__-__-__"
        />
        <input
          v-model="isAdressValue"
          class="input pl-3 p-[5px] pr-3"
          type="text"
          placeholder="Your adress"
        />
      </div>
      <button
        :disabled="buttonDisabled"
        @click="handleSendOrder"
        class="mt-[50px] bg-lime-500 w-80 m-auto rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer transition hover:bg-lime-600 active:bg-lime-700 disabled:cursor-default"
      >
        Order
      </button>
    </div>
  </div>
</template>
<script setup>
import { computed, inject, ref } from 'vue'

defineProps({
  createOrder: Function
})
const emit = defineEmits(['createOrder'])

const { cart, closeOrder } = inject('cart')

const isNameValue = ref('')
const isNumberValue = ref('')
const isAdressValue = ref('')

const formatPhoneNumber = (e) => {
  let phoneNumber = e.target.value.replace(/\D/g, '')
  let formatted = ''

  if (phoneNumber.length > 0) {
    formatted = '(' + phoneNumber.substring(0, 3)
    if (phoneNumber.length > 3) {
      formatted += ') ' + phoneNumber.substring(3, 6)
    }
    if (phoneNumber.length > 6) {
      formatted += '-' + phoneNumber.substring(6, 8)
    }
    if (phoneNumber.length > 8) {
      formatted += '-' + phoneNumber.substring(8, 10)
    }
    if (phoneNumber.length > 10) {
      formatted += '-' + phoneNumber.substring(10, 12)
    }
  }

  isNumberValue.value = formatted
}

const buttonDisabled = computed(() => {
  const isEmpty =
    isNameValue.value.length > 1 &&
    isNumberValue.value.replace(/\D/g, '').length === 12 &&
    isAdressValue.value.length > 1

  return !isEmpty
})
const handleSendOrder = async () => {
  const orderData = {
    name: isNameValue.value,
    phoneNumber: isNumberValue.value,
    adress: isAdressValue.value
  }
  await emit('createOrder', orderData)
}
</script>
<style scoped>
.input {
  outline: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.322);
}
</style>
