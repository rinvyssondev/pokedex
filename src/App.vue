<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter" id="fundoPoke">
      <img src="./assets/pokemon.png" />
      <h4 class="is-size-4">Pokedex</h4>
      <input
        class="input is-rounded"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
      />
      <button
        class="button is-fullwidth is-success"
        id="buscaBtn"
        @click="buscar"
      >
        Buscar
      </button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon_p :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon_p from "./components/Pokemon_p";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegou a lista de pokemons");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokemon_p,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        return (this.filteredPokemons = this.pokemons);
      } else {
        return (this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.busca
        ));
      }
    },
  },
  computed: {
    /* // Busca em tempo real
    resultadoBusca: function () {
      if (this.busca == "" || this.busca == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
      }
    },*/
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#buscaBtn {
  margin-top: 2%;
}
</style>
