<template>
  <OrderForm v-if="orderOpen" @create-order="createOrder" />
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

  <div class="bg-white flex flex-col w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        descr="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :descr="`Уважаемый ${orderName}, ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        imageUrl="/order-success-icon.png"
      />
    </div>

    <CartItemList v-if="totalPrice" />
    <div v-if="totalPrice">
      <div class="flex flex-col gap-4 mb-6 my-7">
        <div class="flex gap-2">
          <span>Total:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice - vatPrice }} 원</b>
        </div>
        <div class="flex gap-2">
          <span>Tax 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} 원</b>
        </div>
      </div>
      <button
        :disabled="buttonDisabled"
        @click="() => emit('openOrder')"
        class="bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer transition hover:bg-lime-600 active:bg-lime-700"
      >
        Checkout
      </button>
    </div>
  </div>
</template>
<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'
import axios from 'axios'
import { ref, computed, inject } from 'vue'
import OrderForm from './OrderForm.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  openOrder: Function,
  orderOpen: Boolean
})

const emit = defineEmits(['openOrder'])
const { cart, closeOrder } = inject('cart')
const isCreating = ref(false)
const orderId = ref(null)
const orderName = ref('')

const createOrder = async (orderData) => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://6fbd4cce9fa1e626.mokky.dev/orders', {
      cart: cart.value,
      totalPrice: props.totalPrice.value,
      ...orderData
    })
    cart.value = []
    orderId.value = data.id
    closeOrder()
    orderName.value = orderData.name
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreating.value = false
  }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>
