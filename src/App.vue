<script setup>
import { ref } from 'vue'
import Header from './components/Header.vue'
import PokemonCard from './components/PokemonCard.vue'

let firstPokemonCount = ref(0);
let maxPokemonCount = ref(151);

const region = ref('kanto');

let search = ref('')

function changeRegion(newRegion){
  region.value= newRegion
  RegionSelect();
}

function searchInput(newSearch){
  search.value = newSearch
}

function RegionSelect() {
  switch (region.value) {
    case "kanto":
      firstPokemonCount.value = 0;
      maxPokemonCount.value = 151;
      break;
    case "johto":
      firstPokemonCount.value = 152;
      maxPokemonCount.value = 251;
      break;
    case "hoenn":
      firstPokemonCount.value = 252;
      maxPokemonCount.value = 386;
      break;
    case "sinnoh":
      firstPokemonCount.value = 387;
      maxPokemonCount.value = 493;
      break;
    case "unova":
      firstPokemonCount.value = 494;
      maxPokemonCount.value = 649;
      break;
    case "kalos":
      firstPokemonCount.value = 650;
      maxPokemonCount.value = 721;
      break;
    case "alola":
      firstPokemonCount.value = 722;
      maxPokemonCount.value = 809;
      break;
    case "galar":
      firstPokemonCount.value = 810;
      maxPokemonCount.value = 898;
      break;
    default:
      firstPokemonCount.value = 811;
      maxPokemonCount.value = 1000;
      break;
  }
  search.value = ''
}
</script>
<template>
  <header>
    <Header @region-select="changeRegion" @search-pokemon="searchInput"></Header>
  </header>
  <main>
    <PokemonCard :firstPokemonCount = 'firstPokemonCount' :maxPokemonCount = 'maxPokemonCount' :searchValue = 'search' ></PokemonCard>
  </main>
</template>
<!-- iiiiiiiii-->
<style>
  :root{
    --backgroundColor: #007369;
    --cardsColor: #02A676;
    --textColor: #f2f2f2;
    --headerColor: #003840;
    --blackColor: #003840
  }
  body{
    background-color: var(--backgroundColor);
    position: relative;
    min-height: 100vh;
    width: 100%;
  }
</style>
