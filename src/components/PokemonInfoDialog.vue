<script setup>
import { defineEmits, defineProps, onMounted, ref } from "vue";

const emit = defineEmits(["close"]);
let selectedPokemon = ref([]);
let selectedPokemonDetails = ref([]);
let typesQuant = ref();

const props = defineProps({
  selectedPokemonId: Number,
});

onMounted(async () => {
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
    fetchSpecies()
    const typesQuant = data.types.length > 1 ? "doubletype" : "monotype";
  } catch (error) {
    console.error(error);
  }
});

async function fetchSpecies(){
  try {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${props.selectedPokemonId}/`);
    const data = await response.json();
    selectedPokemonDetails.value = {
      description: data.flavor_text_entries.find((entry) => entry.language.name === 'en').flavor_text,
    };
    console.log(data);
  } catch (error) {
    console.error(error);
  }
};
  


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
            <i class='bx bx-x'></i>
          </div>
          <div class="modal-header">
              <img :src="selectedPokemon.image" alt="" class="pokemonImg" />
            <div class="pokemonInfo">
              <div class="baseInfo">
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
            <slot name="header">  </slot>
          </div>

          <div class="modal-body">
            <slot name="body"> {{ selectedPokemonDetails.description }} </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              
            </slot>
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
  width: 55%;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: var(--headerColor);
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  color: white;
}

.closeButton{
  display: flex;
  justify-content: end;
  font-size: 30px;
}

.modal-header {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  flex-wrap: wrap;
}

.pokemonImg {
  background-color: var(--cardsColor);
  border-radius: 60px;
  width: 50%;
}
.pokemonTypes {
  display: flex;
  align-items: center;
  align-content: center;
  justify-content: center;
  gap: 5px;
}
.pokemonTypes img{
  width: 5em;
}

.pokemonInfo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2em;
}
.pokemonName {
  font-size: 4em;
  font-weight: bold;
}
.pokemonId {
  font-size: 2.5em;
  color: #B3B3B3;
  font-weight: bold;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

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
@media (max-width:1024px){
  .pokemonTypes img{
    width: 4em;
  }
  .pokemonName {
    font-size: 3em;
  }
  .pokemonId {
    font-size: 1.5em;
  }
}

@media (max-width:770px){
  .pokemonName {
    font-size: 2.5em;
  }
  .pokemonId {
    font-size: 1.5em;
  }
}
@media (max-width:430px){
  .modal-container{
    width: 80%;
  }
  .pokemonImg{
    width: 80%;
  }
  .pokemonTypes img{
    width: 3.5em;
  }
  .pokemonInfo{
    gap: 1em;
    flex-direction: row;
    justify-content:space-evenly;
    gap: 35px;
  }
  .pokemonTypes{
    flex-direction: column;
  }
  .pokemonName {
    font-size: 2em;
  }
  .pokemonId {
    font-size: 1em;
  }
}
</style>
