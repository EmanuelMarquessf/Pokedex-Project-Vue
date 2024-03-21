<script setup>
import { ref, defineProps, onMounted } from "vue";

const props = defineProps({
  evolutionChain: String,
  pokemonEvolvesFrom: String,
  pokemonDetails: Object,
});

let selectedPokemon = ref([]);
let selectedPokemonDetails = ref([]);
let evolutionChain = [];
let teste;
let evolutionDetailsArray = ref([]);

onMounted(async () => {
  setTimeout(() => {
    dataLoad().catch((error) => console.log(error));
  }, 3000);
});

async function dataLoad() {
  const response = await fetch(props.evolutionChain);
  const data = await response.json();
  let loadedPokemon;

  evolutionChain = [];

  evolutionChain.push(data.chain.species ? data.chain.species : ""),
    evolutionChain.push(
      data.chain.evolves_to[0].species != undefined
        ? data.chain.evolves_to[0].species
        : ""
    );
  evolutionChain.push(
    data.chain.evolves_to[0].evolves_to[0] != undefined
      ? data.chain.evolves_to[0].evolves_to[0].species
      : undefined
  );

  for (const evolve of evolutionChain) {
    let pokemonId = evolve.url
      .replace("https://pokeapi.co/api/v2/pokemon-species/", "")
      .replace("/", "");
    loadedPokemon = await fetchPokemon(pokemonId);
    evolutionDetailsArray.value.push(loadedPokemon);
  }
  console.log(evolutionDetailsArray);
}

async function fetchPokemon(pokemonId) {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${pokemonId}`
    );
    const data = await response.json();

    selectedPokemonDetails = await fetchPokemonDetails(pokemonId);

    const selectedPokemon = {
      id: data.id,
      pokedexNumber: String(data.id).padStart(3, "0"),
      name: data.name.replace("-", " "),
      image: data.sprites.other["official-artwork"].front_default,
      sprite: data.sprites.front_default,
      types: data.types.map((type) => ({
        name: type.type.name,
        icon: `../../public/types/${type.type.name}.svg`,
      })),
    };

    console.log("aa");
    console.log(data);
    console.log("aa");

    return selectedPokemon;
  } catch (error) {
    console.error(error);
  }
}

async function fetchPokemonDetails(pokemonId) {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon-species/${pokemonId}`
    );
    const data = await response.json();

   // const evolutionDetails = await fetchEvolutionDetails(data.evolution_chain.url);


    const pokemonDetails = {
      evolvesFrom : data.evolves_from_species,
      evolutionChain: data.evolution_chain.url
    }

    console.log('pokemonDetails')
    console.log(data);
    return pokemonDetails;

  } catch (error) {
    console.error(error);
  }
}

async function fetchEvolutionDetails(urlEvolutionChain) {
  try {
    const response = await fetch(urlEvolutionChain);
    const data = await response.json();

    console.log('pokemonDetails')
    console.log(data);
    return pokemonDetails;

  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
  <div class="container">
    <div class="pokemonCards" v-for="evolution of evolutionDetailsArray">
      <div class="pokemonData">
        <p>{{evolution.name}}</p>
        <img alt="" :src="evolution.sprite" />
        <div class="pokemonTypes" v-for="type of evolution.types">
          <img :src="type.icon" alt="">
        </div>
      </div>
      <span class="evolutionForm">
        teste
      </span>
    </div>
  </div>
</template>

<style scoped>
  .container{
    background-color: var(--black);
    border-radius: 10px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px 30px;
  }
  .pokemonCards{

  }
  .pokemonTypes{
    display: flex;
    flex-direction: row;
    gap: 1rem;
  }
</style>
