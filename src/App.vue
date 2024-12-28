<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  components: { PokemonCard },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {
    async fetchPokemons() {
      try {
        const randomIds = Array.from({ length: 20 }, () => Math.floor(Math.random() * 1010) + 1);
        const pokemonData = await Promise.all(
          randomIds.map(async (id) => {
            const detail = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`);
            return {
              name: detail.data.name,
              image: detail.data.sprites.front_default || "https://via.placeholder.com/150",
            };
          })
        );
        this.pokemons = pokemonData;
        console.log(this.pokemons);
      } catch (error) {
        console.error("Error fetching Pokémon data:", error);
      }
    },
    incrementDiscoveredCount() {
      this.discoveredCount += 1;
    },
  },
  mounted() {
    this.fetchPokemons();
  },
};
</script>

<template>
  <div id="app">
    <img
      src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1200px-International_Pok%C3%A9mon_logo.svg.png"
      class="pokemon-logo" alt="Pokemon logo" />
    <h1 class="pokemon-title">¿Quién es ese Pokémon?</h1>
    <div class="counter">Pokémon descubiertos: {{ discoveredCount }}</div>
    <div class="pokemon-grid">
      <PokemonCard v-for="pokemon in pokemons" :key="pokemon.name" :pokemon="pokemon"
        @pokemon-discovered="incrementDiscoveredCount" />
    </div>
  </div>
</template>

<style>
/* Variables de color */
:root {
  --pokedex-red: #e63946;
  --pokedex-black: #343a40;
  --pokedex-gray: #6c757d;
  --pokedex-white: #f8f9fa;
  --pokedex-blue: #457b9d;
  --pokedex-yellow: #ffca3a;
}

body {
  margin: 0;
  padding: 0;
  background-color: var(--pokedex-red);
  color: var(--pokedex-white);
  font-family: Arial, sans-serif;
}

#app {
  text-align: center;
  padding: 20px;
}

.pokemon-logo {
  width: 300px;
  margin: 20px auto;
}

.pokemon-title {
  color: var(--pokedex-yellow);
  font-size: 3rem;
  font-weight: bold;
  -webkit-text-stroke: 2px var(--pokedex-blue);
  text-shadow: 2px 2px var(--pokedex-black);
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.counter {
  margin: 20px;
  font-size: 1.5rem;
  background-color: var(--pokedex-gray);
  padding: 10px 20px;
  border-radius: 10px;
  display: inline-block;
  box-shadow: 0 4px var(--pokedex-black);
}
</style>
