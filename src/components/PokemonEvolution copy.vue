<script setup>
import { ref, defineProps, onBeforeMount, watch} from "vue";

const props = defineProps({   
  evolutionChain: String,
  pokemonEvolvesFrom: String,
  pokemonDetails: Object,
});

let selectedPokemonEvolves = ref([]);
let selectedPokemon = [];
let evolutionInformations = []
let evolutionDetails;

watch(
  () => props.pokemonDetails.evolutionChain,
  async () => {
    try {
      const response = await fetch(`${props.pokemonDetails.evolutionChain}`);
      const data = await response.json();
      const selectedPokemonEvolves = data.chain.evolves_to;

      let evolutionDetailsArray = [];
      let loadedPokemon = [];
        
          for (const evolve of selectedPokemonEvolves) {
            let pokemonId = evolve.species.url.replace('https://pokeapi.co/api/v2/pokemon-species/','').replace('/','')
            do {
              loadedPokemon =  await fetchPokemon(evolve, pokemonId);
              evolutionDetailsArray.push(loadedPokemon);
              pokemonId++;
              console.log(pokemonId)
            } while (!loadedPokemon.finalForm);
          }
          

          //let pokemonId = selectedPokemonEvolves[0]. props.pokemonEvolvesFrom.replace('https://pokeapi.co/api/v2/pokemon-species/','').replace('/','')
          
          let pokemonId = props.pokemonEvolvesFrom.replace('https://pokeapi.co/api/v2/pokemon-species/','').replace('/','')
          
          loadedPokemon = await fetchPokemon(selectedPokemonEvolves[0], pokemonId);

          while(loadedPokemon.evolvesFrom != null){
            pokemonId--;
            loadedPokemon = await fetchPokemon(selectedPokemonEvolves[0], pokemonId);
          }

          evolutionDetailsArray.unshift(loadedPokemon)
          
          console.log(loadedPokemon)
          console.log(pokemonId)

      console.log('....')
      console.log(evolutionDetailsArray)
      console.log('....')

    } catch (error) {
      console.error(error);
    }
  }
);
async function fetchPokemonDetails(pokemonId) {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon-species/${pokemonId}`
    );
    const data = await response.json();
    const pokemonDetails = {
      evolvesFrom : data.evolves_from_species,      
    }
    return pokemonDetails
    
  } catch (error) {
    console.error(error);
  }
}

async function fetchPokemon(evolve, pokemonId) {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${pokemonId}`
    );
    const data = await response.json();
    let evolvesFrom = await fetchPokemonDetails(pokemonId)
    const selectedPokemon = {
      id: data.id,
      pokedexNumber: String(data.id).padStart(3, "0"),
      name: data.name.replace('-', ' '),
      image: data.sprites.other["official-artwork"].front_default,
      sprite: data.sprites.front_default,
      types: data.types.map((type) => ({
        name: type.type.name,
        icon: `../../public/types/${type.type.name}.svg`,
      }
      )),
      nextEvolution: {
        name: evolve.evolves_to[0].species.name,
        trigger: evolve.evolves_to[0].evolution_details[0].trigger.name,
        level: evolve.evolves_to[0].evolution_details[0].min_level,
      },
      finalForm: evolve.evolves_to[0].species.name ==  data.name ? true : false,
      evolvesFrom: evolvesFrom,
    };
    console.log('evolvesFrom')
    console.log(selectedPokemon.evolvesFrom)
    console.log(evolve.species.name)
    console.log(selectedPokemon.name)

    return selectedPokemon;
  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
  <div class="container">
    <div class="pokemonImg">
      <img :src="selectedPokemon.image" alt="" />
    </div>
  </div>
</template>

<style scoped></style>
