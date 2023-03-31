<script setup>
import { ref } from "vue";
import axios from "axios";

const searchTerm = ref("");
const searchResults = ref([]);
const apiUrl = "https://pokeapi.co/api/v2/pokemon/";

async function searchPokemon() {
  try {
    const response = await axios.get(apiUrl + searchTerm.value.toLowerCase());
    searchResults.value = [response.data];
  } catch (error) {
    console.error(error);
    searchResults.value = [];
  }
}

function handleSubmit(event) {
  event.preventDefault();
  searchPokemon();
}
</script>

<template>
  <div>
    <form @submit="handleSubmit">
      <h1 for="search">Search for a Pokemon:</h1>
      <div>
        <input id="search" type="text" v-model="searchTerm" />
      </div>
      <div>
        <button class="btn btn-outline-primary btn-sm mt-2">Search</button>
      </div>
    </form>
    <div v-if="searchResults.length === 0">
      <h2 class="mt-2">No hay resultados</h2>
    </div>
    <div v-else>
      <h2>Search Results</h2>
      <ul class="list-group">
        <li class="list-group-item" v-for="result in searchResults" :key="result.name">
          <router-link :to="`/pokemons/${result.name}`">
          <img :src="result.sprites?.front_default" alt="">
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>


<style scoped>
h1 {
  text-align: center;
  font-size: 30px;
  font-family: 'Press Start 2P', cursive;
}

</style>