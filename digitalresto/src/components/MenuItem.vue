<script setup>
const props = defineProps({
  img: String,
  name: String,
  price: Number,
  description: String
})

const emit = defineEmits(['addOrder']);
const addOrder = () => emit('addOrder', {name: props.name, price: props.price})
const imgsrc = props.img
console.log('image source: ' + imgsrc)
</script>

<template>
  <div
    class="xs:w-1/3 sm:w-1/2 xl:w-1/3 h-fit border bg-white rounded-2xl mx-6 my-2 pb-8"
  >
    <div class="w-full h-fit border-solid border-0 border-red-400">
      <img :src="imgsrc" class="rounded-t-2xl w-full" />
      <p class="font-mono font-bold text-2xl mt-2 text-center">{{ name }}</p>
    </div>

    <div class="px-4 py-1 mt-0 w-full lg:h-[20vh] sm:h-fit md:h-fit xs:h-fit border-solid border-0 border-indigo-600">
      <p class="py-1 font-sans xs:text-xs sm:text-xs md:text-sm xl:text-sm 2xl:text-base text-justify">{{ description }}</p>
    </div>

    <div class="flex flex-col items-center justify-evenly h-[10vh] border-solid border-0 border-red-600">
      <p class="font-mono text-center">{{ price }}</p>
      <div class="w-full flex justify-center">
        <button @click="addOrder"
          class="bg-lime-200 hover:bg-lime-400 text-black font-bold py-2 px-4 rounded-full mx-auto"
        >
          Order
        </button>
      </div>
    </div>
  </div>
</template>
