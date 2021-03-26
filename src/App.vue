<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/Pokedex_logo.png" alt="" />
      <br />
      <div id="busca" class="columns">
        <input
          type="text"
          class="input column is-link is-outlined"
          placeholder="Buscar pelo Nome"
          v-model="busca"
        />
        <button class="button is-normal is-warning" @click="buscar">
          Buscar
        </button>
      </div>
      <div v-for="(pokemon, index) in pokemonsFiltrados" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      pokemonsFiltrados: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.pokemonsFiltrados = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      this.pokemonsFiltrados = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.pokemonsFiltrados = this.pokemons;
      } else {
        this.pokemonsFiltrados = this.pokemons.filter(
          (pokemon) => pokemon.name.indexOf(this.busca.toLowerCase()) != -1
        );
      }
    },
  },
};
</script>
  
<style>
#app {
  font-family: "Acme", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #90ee90;
}
#busca {
  margin-top: 2%;
}
</style>
