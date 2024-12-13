<script setup>
import {ref} from 'vue'
import axios from 'axios'

const key = '13e27bcd'

const leftMovie = ref(null)
const rightMovie = ref(null)
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
  if(side == 'left'){
    leftMovie.value = await fetch(inputLeft.value)
  } else if (side == 'right'){
    rightMovie.value = await fetch(inputRight.value)
  }
}

</script>

<template>

  <div class="">
    <div class="bg-slate-300 pt-5 pb-5 text-center text-green-500 font-bold">
      <h1>Movie Comparision</h1>
      <h2>With Vue.js</h2>
    </div>

    <div class="flex flex-row justify-evenly mt-4">

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

    <div class="flex flex-row">

      <p v-if="!leftMovie && !rightMovie" class="text-center w-full text-green-500 font-bold">
        Start searching in any side!
      </p>

      <div v-if="leftMovie" class="flex flex-col w-6/12 items-center mt-5">
        <img :src="leftMovie.Poster" alt="" >
        <div class="w-6/12">

          <p>Director: {{leftMovie.Director}}</p>
          <p>Genre: {{leftMovie.Genre}}</p>
          <p>{{leftMovie.Plot}}</p>
          <p>Meta Score: {{leftMovie.Metascore}}</p>
          <div>
            <p>Awards: {{leftMovie.Awards}}</p>
            <p>Box Office: {{leftMovie.BoxOffice}}</p>
          </div>
        </div>
      </div>

      <div v-if="rightMovie" class="flex flex-col w-6/12 items-center mt-5">
        <img :src="rightMovie.Poster" alt="" >
        <div class="w-6/12">

          <p>Director: {{rightMovie.Director}}</p>
          <p>Genre: {{rightMovie.Genre}}</p>
          <p>{{rightMovie.Plot}}</p>
          <p>Meta Score: {{rightMovie.Metascore}}</p>
          <div>
            <p>Awards: {{rightMovie.Awards}}</p>
            <p>Box Office: {{rightMovie.BoxOffice}}</p>
          </div>
        </div>
      </div>
    </div>

    <div>

    </div>
  </div>
  
</template>

<style scoped>

</style>
