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
  search.value = newSearch;
}

function RegionSelect() {
  switch (region.value) {
    case "kanto":
      firstPokemonCount.value = 0;
      maxPokemonCount.value = 151;
      break;
    case "johto":
      firstPokemonCount.value = 151;
      maxPokemonCount.value = 251;
      break;
    case "hoenn":
      firstPokemonCount.value = 251;
      maxPokemonCount.value = 386;
      break;
    case "sinnoh":
      firstPokemonCount.value = 386;
      maxPokemonCount.value = 493;
      break;
    case "unova":
      firstPokemonCount.value = 493;
      maxPokemonCount.value = 649;
      break;
    case "kalos":
      firstPokemonCount.value = 649;
      maxPokemonCount.value = 721;
      break;
    case "alola":
      firstPokemonCount.value = 721;
      maxPokemonCount.value = 809;
      break;
    case "galar":
      firstPokemonCount.value = 809;
      maxPokemonCount.value = 898;
      break;
    case "paldea":
      firstPokemonCount.value = 905;
      maxPokemonCount.value = 1010;
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
<!-- teste -->
<style>
  :root{
    --backgroundColor: #007369;
    --cardsColor: #02A676;
    --textColor: #f2f2f2;
    --headerColor: #003840;
    --darkGreen: #003840;
    --black: #1d1b31;
  }
  body{
    background-color: var(--backgroundColor);
    position: relative;
    min-height: 100vh;
    width: 100%;
  }
</style>
