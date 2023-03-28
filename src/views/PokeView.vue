<script setup>
import { useRoute, useRouter } from "vue-router";
import { useGetData } from "../composables/getData";
import { useFavoritosStore } from "../store/favoritos";





const route = useRoute();
const router = useRouter();

const useFavoritos = useFavoritosStore();

const { add, findPoke } = useFavoritos;

const { getData, data, loading, error } = useGetData();

const pokeList = () => {
  router.push("/pokemons");
};

const pokeSearch = () => {
  router.push("/search");
};

getData(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`);

const moves = data?.moves?.map((move) => move.move.name).join(", ");
</script>

<template>
  <p v-if="loading">Loading...</p>
  <div class="alert alert-danger mt-2" v-if="error">No existe el pokemon</div>
  <div v-if="data">
    <h1>{{ $route.params.name.toUpperCase() }}</h1>
    <div>
      <h4>Normal version</h4>
      <img :src="data.sprites?.front_default" alt="" />

      <img :src="data.sprites?.back_default" alt="" />
    </div>
    <div>
      <h4>Varicolor</h4>
      <img :src="data.sprites?.front_shiny" alt="" />

      <img :src="data.sprites?.back_shiny" alt="" />
    </div>
    <p>Height: {{ data.height }} Weight: {{ data.weight }}</p>
    <p>Types: {{ data.types?.map((type) => type.type.name).join(", ") }}</p>
    <p>
      Abilities:
      {{ data.abilities?.map((ability) => ability.ability.name).join(", ") }}
    </p>
    <button
      :disabled="findPoke(data.name)"
      class="btn btn-primary mb-2"
      @click="add(data)"
    >
      Agregar Favoritos
    </button>
  </div>

  <button @click="pokeList" class="btn btn-outline-primary me-4">Go to the list</button>
  <button @click="pokeSearch" class="btn btn-outline-primary">Return to Search</button>
</template>
