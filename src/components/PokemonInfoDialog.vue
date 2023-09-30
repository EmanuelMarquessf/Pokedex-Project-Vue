<script setup>
import {
  defineEmits,
  defineProps,
  defineAsyncComponent,
  onBeforeMount,
  onMounted,
  ref,
} from "vue";

const PokemonStats = defineAsyncComponent(() => import("./PokemonStats.vue"));
const PokemonEvolution = defineAsyncComponent(() => import("./PokemonEvolution.vue"));

const emit = defineEmits(["close"]);
let selectedPokemon = ref([]);
let selectedPokemonDetails = ref([]);
let typesQuant = ref();
let showStats = ref(false);

const props = defineProps({
  selectedPokemonId: Number,
});

onBeforeMount(async () => {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${props.selectedPokemonId}/`
    );
    const data = await response.json();
    selectedPokemon.value = {
      id: data.id,
      pokedexNumber: String(data.id).padStart(3, "0"),
      name: capitalizeFirstLetter(data.name),
      height: data.height,
      weight: data.weight,
      abilities: data.abilities.map((ability) => ability.ability.name),
      stats: data.stats,
      image: data.sprites.other["official-artwork"].front_default,
      sprite: data.sprites.front_default,
      animatedSprite:
        data.sprites.versions["generation-v"]["black-white"].animated
          .front_default,
      types: data.types.map((type) => ({
        name: type.type.name,
        icon: `../../public/types/${type.type.name}.svg`,
      })),
      speciesUrl: data.species.url,
    };
    console.log(data);
    fetchSpecies();
  } catch (error) {
    console.error(error);
  }
});

onMounted(async () => {
  showStats.value = true;
});

async function fetchSpecies() {
  try {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon-species/${props.selectedPokemonId}/`
    );
    const data = await response.json();
    selectedPokemonDetails.value = {
      description: data.flavor_text_entries.find(
        (entry) => entry.language.name === "en"
      ).flavor_text,
      genera: data.genera[7].genus,
      capture_rate: data.capture_rate,
      shape: data.shape.name,
      evolutionChain: data.evolution_chain.url,
    };
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

function capitalizeFirstLetter(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

</script>

<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="closeButton" @click="emit('close', false)">
            <i class="bx bx-x"></i>
          </div>
          <div class="modal-header">
            <div class="pokemonImg">
              <img :src="selectedPokemon.image" alt="" />
            </div>
            <div class="pokemonInfo">
              <div class="baseInfo">
                <div>
                  <p class="pokemonName">{{ selectedPokemon.name }}</p>
                  <p class="pokemonId">#{{ selectedPokemon.pokedexNumber }}</p>
                </div>
                <div class="pokemonTypes">
                  <img
                    v-for="type in selectedPokemon.types"
                    :src="type.icon"
                    alt=""
                  />
                </div>
              </div>
              <div class="pokemonDescription">
                {{ selectedPokemonDetails.description }}
              </div>
            </div>
            <slot name="header"> </slot>
          </div>
          <div class="modal-body">
            <div class="pokemonData">
              <ul>
                <li>
                  <p class="liTitle">Genera:</p>
                  <p>{{ selectedPokemonDetails.genera }}</p>
                </li>
                <li>
                  <p class="liTitle">Shape:</p>
                  <p class="pShape">{{ selectedPokemonDetails.shape }}</p>
                </li>
                <li>
                  <p class="liTitle">Heigth:</p>
                  <p>{{ (selectedPokemon.height / 10).toFixed(2) }} m</p>
                </li>
                <li>
                  <p class="liTitle">Weigth:</p>
                  <p>{{ (selectedPokemon.weight / 10).toFixed(2) }} kg</p>
                </li>
                <li>
                  <p class="liTitle">Capture Rate:</p>
                  <p>
                    {{ selectedPokemonDetails.capture_rate }} ({{
                      (
                        (selectedPokemonDetails.capture_rate / 255) *
                        100
                      ).toFixed(2)
                    }}%)
                  </p>
                </li>
              </ul>
            </div>
            <PokemonStats :pokemonStats="selectedPokemon.stats"></PokemonStats>
          </div>
          <div class="modal-footer">
            <PokemonEvolution :evolutionChain="selectedPokemonDetails.evolutionChain"></PokemonEvolution>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 65%;
  height: 100vh;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: var(--headerColor);
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  color: white;
  overflow-y: scroll;
}

.closeButton {
  display: flex;
  justify-content: end;
  font-size: 30px;
}

.modal-header {
  display: flex;
  flex-direction: row;
  justify-content: left;
  background-color: var(--black);
  border-radius: 65px;
  flex-wrap: wrap;
  gap: 20px;
}

.pokemonImg {
  background-color: var(--cardsColor);
  border-radius: 65px;
  object-fit: cover;
}

.pokemonImg img {
  width: 100%;
  height: 100%;
}
.baseInfo {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.pokemonInfo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 50px;
  width: 45%;
  flex-wrap: wrap;
}
.pokemonTypes {
  display: flex;
  align-items: center;
  align-content: center;
  gap: 10px;
}
.pokemonTypes img {
  width: 5em;
}

.pokemonName {
  font-size: 5em;
  font-weight: bold;
  font-family: "Roboto", sans-serif;
  font-weight: 800;
}
.pokemonId {
  font-size: 2.5em;
  color: #b3b3b3;
  font-weight: bold;
  font-family: "Roboto", sans-serif;
}
.pokemonDescription {
  font-size: 2rem;
  text-align: justify;
  font-family: "Roboto", sans-serif;
  font-weight: 500;
  width: 100%;
}
.modal-body {
  margin: 30px 0;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  gap: 1rem;
  flex-wrap: wrap;
}
.pokemonData {
  background-color: var(--black);
  padding: 20px;
  flex-basis: 25rem;
  flex-grow: 1;
  border-radius: 20px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.pokemonData p {
  font-size: 2rem;
}
.pokemonData ul {
  border-radius: 20px;
}
.pokemonData ul li {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  list-style-type: none;
  border-bottom: #f2f2f2 1px solid;
  font-size: 20px;
  line-height: 45px;
  color: #b3b3b3;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
}
.pShape {
  text-transform: capitalize;
}
.liTitle {
  font-weight: bold;
  color: #fff;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

@media (max-width: 1440px) {
  .pokemonData p{
    font-size: 1.5rem;
  }
  
}

@media (max-width: 1490px) {
  .modal-header {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    border-top-left-radius: 65px;
    border-top-right-radius: 65px;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
    flex-grow: 1;
  }
  .pokemonImg {
    width: 100%;
  }
  .pokemonInfo {
    flex-direction: column;
    gap: 30px;
    width: 100%;
  }
  .baseInfo {
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
    flex-grow: 1;
    flex-basis: 150px;
    justify-content: space-around;
  }

  .pokemonDescription {
    padding: 0px 45px 40px 45px;
  }
  .pokemonTypes {
    flex-direction: column;
    justify-content: center;
  }
  .pokemonTypes img {
    width: 7em;
  }
  .pokemonName {
    font-size: 4em;
  }
  .pokemonId {
    font-size: 3em;
  }

}

@media (max-width: 770px) {
  .pokemonName {
    font-size: 2.5em;
  }
  .pokemonId {
    font-size: 1.5em;
  }

  .pokemonData p{
    font-size: 1.5rem;
  }
}
@media (max-width: 600px) {
  .modal-container {
    width: 80%;
  }
  .pokemonImg {
    width: 100%;
  }
  .pokemonTypes img {
    width: 3.5em;
  }
  .pokemonInfo {
    gap: 1em;
    flex-direction: row;
    justify-content: space-evenly;
    gap: 10px;
  }
  .pokemonTypes {
    flex-direction: column;
  }
  .pokemonName {
    font-size: 2em;
  }
  .pokemonId {
    font-size: 1em;
  }
  .pokemonDescription {
    font-size: 1.5rem;
    padding: 0px 30px 30px 30px;
  }

  .pokemonData p {
    font-size: 1rem;
  }
@media (max-width: 425px) {
  .modal-container{
    width: 100%;
  }
  .pokemonInfo{
    gap: 1rem;
  }
  .baseInfo{
    gap: 1rem;
    justify-content: center;
  }
  .pokemonName{
    font-size: 1.5rem;
  }
  .pokemonDescription{
    font-size: 1rem;
  }
}
@media (max-width: 320px) {
  .pokemonName{
    font-size: 1.1rem;
    gap:0.5rem
  }

  .pokemonData{
    font-size: 10px;
  }
}


}
</style>
