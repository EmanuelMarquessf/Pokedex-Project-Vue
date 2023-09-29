<script setup>
import { ref, defineProps, onBeforeMount, watch} from "vue";

const props = defineProps({   
  evolutionChain: String,
});


watch(
  () => props.evolutionChain,
  async () => {
    try {
    const response = await fetch(`${props.evolutionChain}`);
    data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
  }
);

let evolutionChain = ref([]);
let data = ref([])

onBeforeMount(async () => {
  try {
    if(props.evolutionChain){
      const response = await fetch(`${props.evolutionChain}`);
      data = await response.json();
      evolutionChain.value = {
      chain: data.evolves_to.chain.evolves_to[0].species.name,
    }
    console.log(data);
    }
    
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
  <div class="container">
    {{ evolutionChain.chain }}
  </div>
</template>

<style scoped></style>
