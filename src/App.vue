<script setup>
import {ref} from 'vue'
import axios from 'axios'

const key = '13e27bcd'

const leftMovie = ref(null)
const rightMovie = ref(null)
const inputLeft = ref('')
const inputRight = ref('')

const suggestionsleft = ref([]);
const suggestionsright = ref([]);


// fetch movies by typing
const fetch = async(search, side) => {
  try {
    const res = await axios.get('https://www.omdbapi.com/', {
      params: { apiKey: key, s: search}
    })
    console.log(search, side)

    if(side == 'left'){
      suggestionsleft.value = res?.data?.Search
    } else if (side == 'right'){
      suggestionsright.value = res?.data?.Search
    }
    
    // suggestionsright.value = res.data.Search
    return res.data
  } catch (error) {
    
  }
}

const debounce = (func, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), delay);
  };
};

// Debounce the fetchSuggestions function
const debouncedFetchSuggestions = debounce((input, side) => fetch(input, side), 300);

//Fetch selected movies
const fetchSingle = async(s) => {
  try {
    const res = await axios.get('https://www.omdbapi.com/', {
      params: { apiKey: key, t: s}
    })
    console.log(s)
    // leftMovie.value = res.data.Search
    suggestionsleft.value = []
    suggestionsright.value = []
    return res.data
  } catch (error) {
    console.log(error)
  }
}

const handleSubmit = async(side, search) => {
  if(side == 'left'){
    leftMovie.value = await fetchSingle(inputLeft.value)
  } else if (side == 'right'){
    rightMovie.value = await fetchSingle(inputRight.value)
  }
}

const clearSuggestions = () => {
  suggestionsleft.value = [];
  suggestionsright.value = [];
};

</script>

<template>

  <div @click="clearSuggestions" class="">

    <!-- header -->
    <div class="bg-slate-300 pt-5 pb-5 text-center text-green-500 font-bold">
      <h1>Movie Comparision</h1>
      <h2>With Vue.js</h2>
    </div> 

    <!-- main -->
    <div class="flex flex-row justify-evenly mt-4">
      <!-- left -->
      <div @click.stop class="">
      
        <form @submit.prevent="handleSubmit('left')" class="flex">
          <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-l-md">Search</button>
          <input @input="debouncedFetchSuggestions(inputLeft, 'left')" type="text" v-model="inputLeft" class="border-2 focus:outline-none">
        </form>

        <ul v-if="suggestionsleft.length" class="absolute bg-white border rounded max-h-48 overflow-y-auto">
          <li v-for="(suggestion, index) in suggestionsleft" :key="index" @click="inputLeft = suggestion.Title;"
            class="px-4 py-2 hover:bg-gray-100 cursor-pointer flex flex-row items-center gap-1"
          >
            <img :src="suggestion.Poster" alt="" class="size-20" >
            {{ suggestion.Title }} ({{ suggestion.Year }})
          </li>
        </ul>
      </div>

      <!-- right -->
      <div class="">
        <form @submit.prevent="handleSubmit('right')" class="flex">
          <input @input="debouncedFetchSuggestions(inputRight, 'right')" type="text" v-model="inputRight" class="border-2 focus:outline-none">
          <button type="submit" class="text-gray-700 bg-green-300 p-2 rounded-r-md">Search</button>
        </form>
        <ul v-if="suggestionsright.length" class="absolute bg-white border rounded max-h-48 overflow-y-auto">
          <li v-for="(suggestion, index) in suggestionsright" 
            :key="index" 
            @click="inputRight = suggestion.Title;"
            class="px-4 py-2 hover:bg-gray-100 cursor-pointer flex flex-row items-center gap-1">
            <img :src="suggestion.Poster" alt="" class="size-20" >
            {{ suggestion.Title }} ({{ suggestion.Year }})
          </li>
        </ul>
      </div>
    </div>

    <!-- right -->
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
    
  </div>
  
</template>

<style scoped>

</style>
