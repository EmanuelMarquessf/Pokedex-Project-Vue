<script setup>
import { ref, defineProps } from "vue";

const props = defineProps({
  pokemonStats: Array,
});

const stats = ['Hp', 'Atk', 'Def', 'Sp.Atk', 'Sp.Def', 'Speed'];
let i=0;

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
            <p>{{ stats[i++]}}:</p>
            <p>{{stat.base_stat}}</p>
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
  width: 45%;
}

.tablePokemonStats{
  background-color: var(--black);
  border-radius: 15px;
  padding: 5px;
  width: 100%;
}

.tdTitleBar{
  display: flex;
  align-items: center;
  justify-content:space-between;
  padding: 3px;
  text-transform: capitalize;
  max-height: 35px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 22px;
  color: white;
}

.tdTitleBar p{
  color: black;
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

@media(max-width:1440px){
  .divPokemonStats{
    width: 100%;
  }
}

</style>
