<script setup>
const emit = defineEmits(['gotoView'])
const gotoOrders = () => emit('gotoView', 'orders')
const gotoMenu = () => emit('gotoView', 'menu')
</script>
<template>
  <div
    class="shadow-md sticky z-50 top-0 h-16 flex justify-between items-center bg-lime-200"
  >
    <h1 class="font-pacifico text-3xl">Gourmet Junction</h1>
    <div class="flex">
      <div class="mx-3">
        <p @click="gotoMenu" class="text-2xl hover:underline hover:cursor-pointer">Menu</p>
      </div>
      <div @click="gotoOrders" class="mx-3"><p class="text-2xl hover:underline hover:cursor-pointer">Orders</p></div>
    </div>
  </div>
</template>
