<script setup>
import PokemonInfoDialog from "./PokemonInfoDialog.vue";

import {
  defineProps,
  computed,
  ref,
  onBeforeMount,
  onMounted,
  watch,
} from "vue";

let data = ref([]);
const pokemons = ref([]);
let typesQuant = ref();
let showDialog = ref(false);
let selectedPokemonId = ref("");

const props = defineProps({
  firstPokemonCount: Number,
  maxPokemonCount: Number,
  searchValue: String,
});

//Busca os dados da api
async function fetchData(url) {
  try {
    const response = await fetch(url);
    return await response.json();
  } catch (error) {
    console.log(error);
    return null;
  }
}

onBeforeMount(async () => {
  const apiUrl = "https://pokeapi.co/api/v2/pokemon?limit=1010";
  const response = await fetchData(apiUrl);
  if (response && response.results) {
    data.value = response;
    fetchPokemons();
  }
});

//Busca pelos pokemons
async function fetchPokemons() {
  let initialRange = props.searchValue == "" ? props.firstPokemonCount : 0;
  let finalRange = props.searchValue == "" ? props.maxPokemonCount : 1000;

  if (data.value.results) {
    const pokemonPromises = data.value.results
      .slice(initialRange, finalRange)
      .map(fetchPokemonDetails);

    pokemons.value = (await Promise.all(pokemonPromises)).filter(
      (pokemon) => pokemon !== null
    );
  }
}

//Atraves da função map é percorrido o segundo link da api e é buscado os detalhes de cada pokemon
async function fetchPokemonDetails(pokemon) {
  const response = await fetchData(pokemon.url);
  if (!response) return null;

  const details = response;
  const pokedexNumber = details.id;
  const sprite = details.sprites.front_default;
  const animatedSprite =
    details.sprites.versions["generation-v"]["black-white"].animated
      .front_default;
  const types = details.types.map((type) => ({
    name: type.type.name,
    icon: `/types/${type.type.name}.svg`,
  }));
  const typesQuant = types.length > 1 ? "doubletype" : "monotype";

  return {
    id: details.id,
    name: pokemon.name,
    pokedexNumber: String(pokedexNumber).padStart(3, "0"),
    types,
    sprite,
    animatedSprite,
    typesQuant,
  };
}

watch(
  () => props.maxPokemonCount,
  async () => {
    fetchPokemons();
  }
);

watch(
  () => props.searchValue,
  async () => {
    fetchPokemons();
  }
);

let filtered_pokemons = computed(() => {
  return pokemons.value.filter((pokemon) => {
    return pokemon.name.includes(props.searchValue);
  });
});

function showPokemon(id) {
  selectedPokemonId.value = id;
  showDialog.value = !showDialog.value;
}
</script>

<template>
  <div class="allCards" v-if="props.searchValue == ''">
    <div
      class="card"
      v-for="pokemon in pokemons"
      :v-if="pokemon.name == props.searchValue"
      :key="pokemon.id"
      @click="showPokemon(pokemon.id)"
    >
      <img :src="pokemon.sprite" alt="" class="pokemonIcon" />
      <div class="pokemonInfo">
        <p class="pokemonName">{{ pokemon.name }}</p>
        <p class="pokemonNumber">#{{ pokemon.pokedexNumber }}</p>
      </div>
      <div :class="pokemon.typesQuant">
        <img v-for="type in pokemon.types" :src="type.icon" alt="" />
      </div>
    </div>
  </div>

  <div class="allCards" v-else-if="props.searchValue != ''" @click="">
    <div
      class="card"
      v-for="pokemon in filtered_pokemons"
      :v-if="pokemon.name == props.searchValue"
      :key="pokemon.id"
      @click="showPokemon(pokemon.id)"
    >
      <img :src="pokemon.sprite" alt="" class="pokemonIcon" />
      <div class="pokemonInfo">
        <p class="pokemonName">{{ pokemon.name }}</p>
        <p class="pokemonNumber">#{{ pokemon.pokedexNumber }}</p>
      </div>
      <div :class="pokemon.typesQuant">
        <img
          class="imgType"
          v-for="type in pokemon.types"
          :src="type.icon"
          alt=""
        />
      </div>
    </div>
  </div>

  <PokemonInfoDialog
    v-if="showDialog"
    @close="showDialog = false"
    :selectedPokemonId="selectedPokemonId"
  ></PokemonInfoDialog>
</template>

<style scoped>
@font-face {
  font-family: "Pokemon Classic";
  src: url("../../public/pokemon_classic/Pokemon Classic.ttf")
    format("truetype");
}
.allCards {
  position: absolute;
  width: calc(100% - 240px);
  left: 240px;
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
  justify-content: center;
  align-content: center;
}
.card {
  background-color: var(--cardsColor);
  border-radius: 20px;
  width: 367px;
  height: 100px;
  display: flex;
  justify-content: space-around;
  user-select: none;
  cursor: pointer;
}
.card:hover {
  background-color: var(--darkGreen);
  color: #fff;
  transition: all ease 0.4s;
}

.pokemonInfo {
  font-family: "Pokemon Classic", Arial, sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100px;
}
.monotype {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 36%;
}
.monotype img {
  margin: 0 auto;
}

.doubletype {
  display: flex;
  gap: 5px;
  align-items: center;
  justify-content: center;
}
.pokemonIcon {
  width: 100px;
  height: 100px;
}
.pokemonName {
  font-size: 18px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  text-transform: capitalize;
}
.pokemonNumber {
  font-size: 14px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

@media (max-width: 600px) {
  .allCards {
    width: calc(100% - 80px);
    left: 80px;
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 100px;
  }
  .card {
    width: 90%;
  }
}
@media (max-width: 480px) {
  .doubletype {
    flex-direction: column;
    margin: 20px auto;
  }
  .allCards {
    width: 100%;
    justify-content: center;
    left: 0;
    margin-top: 20px;
  }
  .card {
  }
}
</style>
