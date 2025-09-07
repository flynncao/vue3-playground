<script setup>
import { nextTick, ref } from 'vue'

const balance = ref(500)
const onoff = ref(false)
const datetime = ref(new Date().toLocaleString())
const discount = 0.7
const list = [
  { id: 1, name: 'apple', price: 3.2 },
  { id: 2, name: 'banana', price: 2.5 },
  { id: 3, name: 'orange', price: 4.1 },
]

const purchase = async (price) => {
  setTimeout(() => {
    console.log('Wait 0 seconds.')
    console.log('setTimeout', document.querySelector('#currentBalance').innerHTML)
  }, 0)
  balance.value -= getRealPrice(price)
  console.log('Purchase successful!')

  getMembershipDataAsync().then((data) => {
    console.log('Membership data:', data)
  })
  getExternalData().then((data) => {
    console.log('External data:', data)
  })

  getLocalData().then((data) => {
    console.log('Local data:', data)
  })

  nextTick(() => {
    console.log('Wait for next tick.')
    datetime.value = new Date().toLocaleString()
    console.log('nextTick', document.querySelector('#currentBalance').innerHTML)
  })
}

const getLocalData = () => {
  return new Promise((resolve) => {
    fetch('members.json')
      .then((response) => response.text())
      .then((json) => {
        resolve(json)
      })
  })
}

const getExternalData = () => {
  return new Promise((resolve) => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then((response) => response.json())
      .then((json) => {
        resolve(json)
      })
  })
}

const getMembershipDataAsync = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ member: 'Gold', points: 12345 })
    }, 1000)
  })
}

const getRealPrice = (price) => {
  return price * discount
}

const reset = () => {
  balance.value = 500
}
</script>
<template>
  <div class="about">
    <table>
      <thead>
        <h2>Current Time: {{ datetime }}</h2>
      </thead>
      <tbody>
        <tr>
          <td>Your balance is:</td>
          <td id="currentBalance">{{ balance.toFixed(2) }}</td>
        </tr>
        <tr v-for="item in list" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.price }}</td>
          <td><button @click="purchase(item.price)">Purchase</button></td>
        </tr>
        <tr>
          <td><button @click="reset">Reset</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
.about-btn {
  margin-right: 5px;
}
</style>
