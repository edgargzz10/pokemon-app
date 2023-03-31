<script setup>
import { RouterLink } from "vue-router";
import { useGetData } from "@/composables/getData";

const { data, getData, loading, error } = useGetData();

getData("https://pokeapi.co/api/v2/pokemon/");
</script>

<template>
  <h1>Pokemons</h1>
  <p v-if="loading">Cargando informacion</p>
  <div class="alert alert-danger mt-2" v-if="error">{{ error }}</div>
  <div v-if="data">
    <ul class="list-group">
      <li class="list-group-item" v-for="poke in data.results">
        <router-link :to="`/pokemons/${poke.name}`">{{
          poke.name
        }}</router-link>
      </li>
    </ul>
    <div class="mt-2">
        <button :disabled="!data.previous" class="btn btn-outline-success me-2" @click="getData(data.previous)">Pevious</button>
        <button :disables="!data.next" class="btn btn-outline-primary" @click="getData(data.next)">Next</button>
    </div>
</div>
</template>


<style scoped>
h1 {
  text-align: center;
  font-size: 30px;
  font-family: 'Press Start 2P', cursive;
}

li {
  
  font-family: 'Press Start 2P', cursive;
}

</style>