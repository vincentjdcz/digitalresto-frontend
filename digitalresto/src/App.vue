<script setup>
import Navbar from './components/Navbar.vue'
import OrderRow from './components/OrderRow.vue'
import { ref } from 'vue'
import MenuItem from './components/MenuItem.vue'

const view = ref('menu')
const orderNumber = ref(1)

const orders = ref([])

async function fetchData() {
  orders.value = []
  const res = await fetch(`https://digitalresto-production.up.railway.app/api/getAllOrders`)
  const ordersjson = await res.json()
  for (const orderjson of ordersjson) {
    if (orderjson.orderId > orderNumber.value) {
      orderNumber.value = orderjson.orderId + 1
    }
    orders.value.push({ id: orderjson.orderId, name: orderjson.item, price: orderjson.price })
  }
}

fetchData()

const menuItems = [
  {
    img: '/assets/pasta.png', //per chatgpt, vite handles assets in public directory differently from those in src. Static assets in the public directory are directly available from root
    //Want to do it this way ^ so that when building for production you won't have issues with not being able to find the images
    name: 'Pasta',
    description:
      'Indulge in our classic Spaghetti Carbonara, a timeless Italian favorite. Al dente spaghetti is perfectly tossed with a rich and creamy sauce made from farm-fresh eggs, grated Pecorino Romano cheese, and crispy pancetta. A touch of freshly ground black pepper and a sprinkle of parsley add the finishing touches to this comforting and flavorful dish.',
    price: 19.99
  },
  {
    img: '/assets/pizza.png',
    name: 'Pizza',
    description:
      "Savor the simplicity and elegance of our Margherita Pizza, a celebration of classic Italian flavors. This delightful pizza features a thin, crispy crust topped with a rich tomato sauce made from sun-ripened tomatoes and fragrant basil. It’s layered with fresh mozzarella cheese that melts to perfection and is finished with a drizzle of extra virgin olive oil. Garnished with fresh basil leaves and a sprinkle of sea salt, our Margherita Pizza is a timeless favorite that's sure to please.",
    price: 17.99
  },
  {
    img: '/assets/salad.png',
    name: 'Salad',
    description:
      'Experience the vibrant flavors of our Mediterranean Garden Salad, a refreshing mix of crisp romaine lettuce, juicy cherry tomatoes, and sliced cucumbers. Kalamata olives and creamy feta cheese add a savory touch, while red onions and roasted red peppers provide a hint of sweetness. Dressed with a light, homemade lemon-herb vinaigrette, this salad is both invigorating and satisfying.',
    price: 12.99
  },
  {
    img: '/assets/burger.png',
    name: 'Burger',
    description:
      'Our Gourmet Classic Cheeseburger features a perfectly grilled, juicy beef patty seasoned to perfection. It’s topped with melted cheddar cheese, crisp lettuce, ripe tomato slices, and tangy pickled onion, offering a delightful blend of flavors and textures. A dollop of our special house sauce enhances the taste, all served on a toasted brioche bun.',
    price: 15.99
  }
]

const changeView = (newView) => {
  if (newView !== view.value) {
    view.value = newView
  }
}

const addOrder = async (newOrd) => {
  const newOrder = { orderId: orderNumber.value++, item: newOrd.name, price: newOrd.price }
  const requestOptions = {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newOrder)
  }
  const response = await fetch(
    'https://digitalresto-production.up.railway.app/api/addOrder',
    requestOptions
  )
  const data = await response.json()

  if (data.error) {
    alert('There was an error submitting the order')
  } else {
    alert('Order placed!')
  }
  //orders.value.push(newOrder)
  fetchData()
}

const removeOrder = async (orderId) => {
  const requestOptions = {
    method: 'DELETE',
    headers: { 'Content-Type': 'application/json' }
  }
  const response = await fetch(
    `https://digitalresto-production.up.railway.app/api/deleteOrder/${orderId}`,
    requestOptions
  )

  fetchData()
  //orders.value = orders.value.filter((order) => order.id !== orderId)
}
</script>

<template>
  <header class="">
    <div class="wrapper">
      <Navbar @gotoView="(value) => changeView(value)" />
    </div>
  </header>

  <main>
    <div class="bg-lime-50 min-h-screen h-fit w-screen flex justify-evenly flex-wrap py-4 px-16">
      <template v-if="view === 'menu'" class="" v-for="item in menuItems">
        <MenuItem
          :img="item.img"
          :name="item.name"
          :price="item.price"
          :description="item.description"
          @addOrder="(newOrder) => addOrder(newOrder)"
        />
      </template>

      <div v-if="view === 'orders'" class="relative overflow-x-auto">
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
          <thead
            class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
          >
            <tr>
              <th scope="col" class="px-6 py-3">ORDER NUMBER</th>
              <th scope="col" class="px-6 py-3">ITEM</th>
              <th scope="col" class="px-6 py-3">PRICE</th>
              <th scope="col" class="px-6 py-3"></th>
            </tr>
          </thead>
          <tbody>
            <template v-for="order in orders">
              <OrderRow
                :orderNumber="order.id"
                :name="order.name"
                :price="order.price"
                @orderReceived="(orderId) => removeOrder(orderId)"
              />
            </template>
          </tbody>
        </table>
      </div>
    </div>
  </main>
</template>
