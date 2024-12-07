<script setup>
import {ref} from 'vue'
import axios from 'axios'

const key = '13e27bcd'

const leftMovie = ref([])
const rightMovie = ref([])

const fetch = async(s) => {
  try {
    const res = await axios.get('https://www.omdbapi.com/', {
      params: { apiKey: key, t: s}
    })
    console.log(res)
  } catch (error) {
    
  }
}

const handleSubmit = (side) => {
  if(side == 'left'){
    fetch(leftMovie.value)
  } else if (side == 'right'){
    fetch(rightMovie.value)
  }
}


</script>

<template>

  <h1 class="text-gray-800">hello</h1>

  <form @submit.prevent="handleSubmit('left')">
    <button type="submit">Submit Left</button>
    <input type="text" v-model="leftMovie">
  </form>

  <form @submit.prevent="handleSubmit('right')">
    <button type="submit">Submit Right</button>
    <input type="text" v-model="rightMovie">
  </form>
  
</template>

<style scoped>

</style>
