<script setup>
import {ref} from 'vue'
import axios from 'axios'

const key = '13e27bcd'

const leftMovie = ref([])
const rightMovie = ref([])

const inputLeft = ref('')

const fetch = async(s) => {
  try {
    const res = await axios.get('https://www.omdbapi.com/', {
      params: { apiKey: key, t: s}
    })
    console.log(res.data)
    return res.data
    // leftMovie.value = res.data
    // rightMovie.value = res.data
  } catch (error) {
    
  }
}

const handleSubmit = async(side, search) => {
  // console.log(search)
  if(side == 'left'){
    // await fetch(search)
    console.log(search)
    leftMovie.value = await fetch(search)
  } else if (side == 'right'){
    fetch(rightMovie.value)
  }
}

</script>

<template>

  <div class="mt-4">
    <h1 class="text-gray-800 text-center mb-5">Movie Comparision</h1>

    <div class="flex flex-row justify-evenly">

      <form @submit.prevent="handleSubmit('left', inputLeft)" class="flex">
        <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-l-md">Search</button>
        <input type="text" v-model="inputLeft" class="border-2 focus:outline-none">
      </form>

      <form @submit.prevent="handleSubmit('right')" class="flex">
        <input type="text" v-model="rightMovie" class="border-2 focus:outline-none">
        <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-r-md">Search</button>
      </form>
    </div>

    <p v-if="!leftMovie.length || !rightMovie.length">Start searching!</p>

      <div v-if="leftMovie.length">
        <div v-for="x in leftMovie" :key='x.imdbID'>
          {{x}}
        </div>
      </div>
    <div>

    </div>
  </div>
  
</template>

<style scoped>

</style>
