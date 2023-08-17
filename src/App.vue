<script setup>
import { ref, onMounted } from 'vue'

const pokemons = ref([])

onMounted(async () => {
  const response = await fetch('https://pokeapi.co/api/v2/pokemon/')
  const jsonResponse = await response.json()

  for (const pokemon of jsonResponse.results) {
    const responsePokemon = await fetch(pokemon.url)
    const jsonResponsePokemon = await responsePokemon.json()
    pokemons.value.push({ ...pokemon, ...jsonResponsePokemon })
  }
  // pokemons.value = jsonResponse.results
  console.log(pokemons.value)
})

</script>

<template>
  <div class="pokemons-group">
    <div v-for="pokemon in  pokemons " :key="pokemon.name" class="pokemon-card">
      <a :href="pokemon.url">
        <img :src="pokemon.sprites.other.dream_world.front_default" :alt="pokemon.name" :title="pokemon.name"
          class="pokemon-image">
        <p class="pokemon-name">{{ pokemon.name }}</p>
        <ul class="pokemon-types">
          <li v-for="types in pokemon.types" :key="types.slot" :class="'pokemon-' + types.type.name">
            {{ types.type.name }}
          </li>
        </ul>
      </a>
    </div>
  </div>
</template>

<style scoped>
.pokemons-group {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-auto-rows: 1fr;
  place-content: center;
  /* align-items: center; */
  gap: 20px;

}

.pokemon-name {
  color: white;
  font-size: 24px;
  text-transform: capitalize;
}

.pokemon-card {
  border: 1px solid #415a77;
  background-color: #1b263b;
  padding: 10px;
  border-radius: 8px;
}

.pokemon-image {
  width: 100%;
  height: 100%;
  max-height: 300px;
}

.pokemon-types {
  list-style: none;
  padding: 0;
  display: flex;
  justify-content: center;
  gap: 10px;
}

.pokemon-types>* {
  color: white;
  border-radius: 10px;
  border: 1px solid white;
  padding-inline: 10px;
  padding-block: 4px;
}

.pokemon-grass {
  background-color: green;
}

.pokemon-fire {
  background-color: red;
}

.pokemon-poison {
  background-color: purple;
}

.pokemon-bug {
  background-color: limegreen;
}

.pokemon-water {
  background-color: blue;
}

.pokemon-flying {
  background-color: yellow;
}

.pokemon-normal {
  background-color: grey;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
