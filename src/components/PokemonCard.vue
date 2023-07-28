<script setup>
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
  const apiUrl = "https://pokeapi.co/api/v2/pokemon?limit=1000";
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
    icon: `../../public/types/${type.type.name}.svg`,
  }));
  const typesQuant = types.length > 1 ? "doubletype" : "monotype";

  return {
    id: pokemon.id,
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
</script>

<template>
  {{ props.searchValue }}
  <div class="allCards" v-if="props.searchValue == ''">
    <div
      class="card"
      v-for="pokemon in pokemons"
      :v-if="pokemon.name == props.searchValue"
      :key="pokemon.id"
      @click=""
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

  <div class="allCards" v-else-if="props.searchValue != ''">
    <div
      class="card"
      v-for="pokemon in filtered_pokemons"
      :v-if="pokemon.name == props.searchValue"
      :key="pokemon.id"
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
    <h1 v-if="filtered_pokemons == 0">Não foi encontrado nenhum pokemon com o nome: '{{ props.searchValue }}'</h1>
  </div>
</template>

<style scoped>
@font-face {
  font-family: "Pokemon Classic";
  src: url("../../public/pokemon_classic/Pokemon Classic.ttf")
    format("truetype");
}
.regionSelect {
  text-align: center;
}

.regionSelect a {
  margin: 10px;
}
.allCards {
  position: absolute;
  height: 100%;
  width: calc(100% - 240px);
  left: 240px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 100px;
}
.card {
  background-color: var(--cardsColor);
  border-radius: 20px;
  width: 367px;
  height: 100px;
  display: grid;
  grid-template-areas: "pokemonIcon pokemonInfo pokemonType";
  margin: 15px;
}
.card:hover {
  background-color: var(--blackColor);
  color: #fff;
  transition: all ease 0.4s;
}

.pokemonInfo {
  font-family: "Pokemon Classic", Arial, sans-serif;
  grid-area: pokemonInfo;
  margin: 25px 0px;
  width: 100px;
}
.monotype {
  margin: 35px auto;
  margin-right: 1px;
  grid-area: pokemonType;
  display: flex;
  width: 128px;
  height: 28px;
}
.monotype img {
  margin: 0 auto;
}

.doubletype {
  margin: 35px 0px;
  margin-right: 1px;
  grid-area: pokemonType;
  display: flex;
  width: 128px;
  height: 28px;
}
.doubletype img {
  margin-right: 5px;
}
.pokemonIcon {
  grid-area: pokemonIcon;
  display: flex;
  width: 100px;
  height: 100px;
}
.pokemonName {
  font-size: 18px;
  padding-left: 0px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
.pokemonNumber {
  font-size: 14px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
</style>
