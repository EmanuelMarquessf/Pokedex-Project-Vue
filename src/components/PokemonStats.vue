<script setup>
import { ref, defineProps } from "vue";

const props = defineProps({
  pokemonStats: Array,
});

console.log(props.pokemonStats);
</script>

<template>
  <div class="divPokemonStats">
    <table class="tablePokemonStats" width="55%">
      <thead>
        <tr>
          <td class="tdTitle" colspan="2">Stat</td>
        </tr>
      </thead>
      <tbody class="tbodyPokemonStats" v-for="stat in props.pokemonStats">
        <tr :class="'tr' + stat.stat.name">
          <td class="tdTitleBar">
            {{ stat.stat.name }}:
            {{stat.base_stat}}
          </td>
          <td class="tdBar">
            <div :style="`width: ${stat.base_stat}px`" :class="`barStats ' ${stat.stat.name}`"></div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.divPokemonStats{
  display: flex;
  justify-content: end;
  align-items: center;
}


.tablePokemonStats{
  background-color: var(--cardsColor);
  color: black;
  border-radius: 15px;
}

.tdTitleBar{
  display: flex;
  align-items: center;
  justify-content:space-between;
  height: 100%;
  padding: 3px;
}

.tdTitle{
  text-align: center;
  font-size: 25px;
  padding: 5px;
}



.tdBar{
width: 60%;
}

.barStats{
  padding: 15px;
  color: white;
  border-radius: 2px;
}

.trhp {
  background-color: #ff5959;
}
.trattack {
  background-color: #f5ac78;
}
.trdefense {
  background-color: #fae078;
}
.trspecial-attack {
  background-color: #9db7f5;
}
.trspecial-defense {
  background-color: #a7db8d;
}
.trspeed {
  background-color: #fa92b2;
}

.hp {
  background-color: #ff0000;
  border: 1px solid #a60000;
}
.attack {
  background-color: #f08030;
  border: 1px solid #9c531f;
}
.defense {
  background-color: #f8d030;
  border: 1px solid #a1871f;
}
.special-attack {
  background-color: #6890f0;
  border: 1px solid #445e9c;
}
.special-defense {
  background-color: #78c850;
  border: 1px solid #4e8234;
}
.speed {
  background-color: #f85888;
  border: 1px solid #a13959;
}
</style>
