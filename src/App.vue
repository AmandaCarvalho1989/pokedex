<template>
  <div id="app">
    <img src="./assets/pokemon.png" alt="" width="200px" />
    <input
      class="input is-rounded"
      type="text"
      placeholder="Search pokemon by name"
      v-model="searchValue"
      style="width: 420px"
    />
    <div class="pokemons">
      <div v-for="(poke, idx) in searchResult" :key="idx" class="poke">
        <Pokemon :name="poke.name" :url="poke.url" />
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
      searchValue: "",
      pokemons: [],
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  computed: {
    searchResult: function () {
      if (this.searchValue == "" || this.searchValue == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter((pokemon) =>
          pokemon.name.includes(this.searchValue)
        );
      }
    },
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
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #fafafa;
}

.pokemons {
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  /* display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-template-rows: auto;
  grid-gap: 16px; */
}

.poke {
  margin: 0 12px;
  width: 220px;
}
</style>
