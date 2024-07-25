<script setup>


const props = defineProps({
    orderNumber: Number,
    name: String,
    price: Number,

})

const emit = defineEmits(['orderReceived']);

const orderReceived = () => emit('orderReceived', props.orderNumber);

</script>

<template>
    <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
              <th
                scope="row"
                class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
              >
                {{orderNumber}}
              </th>
              <td class="px-6 py-4">{{name}}</td>
              <td class="px-6 py-4">{{price}}</td>
              <td class="px-6 py-4"><button @click="orderReceived"
          class="bg-lime-200 hover:bg-lime-400 text-black font-bold py-2 px-4 rounded-full mx-auto"
        >
          Received
        </button></td>
            </tr>
</template>