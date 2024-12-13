<script setup>
import {ref} from 'vue'
import axios from 'axios'

const key = '13e27bcd'

const leftMovie = ref([])
const rightMovie = ref([])
const inputLeft = ref('')
const inputRight = ref('')

const fetch = async(s) => {
  try {
    const res = await axios.get('https://www.omdbapi.com/', {
      params: { apiKey: key, t: s}
    })
    console.log(res.data)
    return res.data
  } catch (error) {
    
  }
}

const handleSubmit = async(side, search) => {
  // console.log(search)
  if(side == 'left'){
    leftMovie.value = await fetch(inputLeft.value)
    // console.log(resLeft)
  } else if (side == 'right'){
    rightMovie.value = await fetch(inputRight.value)
  }
}

</script>

<template>

  <div class="mt-4">
    <h1 class="text-gray-800 text-center mb-5">Movie Comparision</h1>

    <div class="flex flex-row justify-evenly">

      <form @submit.prevent="handleSubmit('left')" class="flex">
        <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-l-md">Search</button>
        <input type="text" v-model="inputLeft" class="border-2 focus:outline-none">
      </form>

      <form @submit.prevent="handleSubmit('right')" class="flex">
        <input type="text" v-model="inputRight" class="border-2 focus:outline-none">
        <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-r-md">Search</button>
      </form>
    </div>

    <!-- <p v-if="!leftMovie.length || !rightMovie.length">Start searching!</p> -->

      <div v-if="leftMovie.length">
        <ul>
          <li v-for="x in leftMovie" :key='x.imdbID'>
            <p>{{x.Title}}</p>
          </li>
        </ul>
      </div>
    <div>

    </div>
  </div>
  
</template>

<style scoped>

</style>
