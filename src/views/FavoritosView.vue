<script setup>
import { storeToRefs } from "pinia";
import { useFavoritosStore } from "../store/favoritos";
import { RouterLink } from "vue-router";

const useFavoritos = useFavoritosStore();

const { favoritos } = storeToRefs(useFavoritos);
const {remove} = useFavoritos
</script>

<template>
  <h1>Favoritos</h1>
  <p v-if="favoritos.length === 0">Sin favoritos</p>
  <ul class="list-group" v-else>
    <li class="list-group-item" v-for="poke in favoritos" :key="poke.id">
      <div v-if="poke.sprites.versions['generation-v']['black-white'].animated.front_default">
        <img :src="poke.sprites.versions['generation-v']['black-white'].animated.front_default" alt="" class="mb-2">
      </div>
      <div v-else>
        <img :src="poke.sprites.front_default" alt="" class="mb-2">
      </div>
      <div>
        <RouterLink :to="`/pokemons/${poke.name}`" class="btn btn-sm btn-primary me-2">Mas informacion</RouterLink>
        <button class="btn btn-sm btn-danger" @click="remove(poke.id)">Eliminar</button>
      </div>
    </li>
  </ul>
</template>
