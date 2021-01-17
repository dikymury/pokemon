<template>
  <div class="about">
    <h1 class="text-green-900 text-xl">{{pokemon.name}}</h1>
    <div v-if="pokemon">{{$route.params.slug}}</div>
  </div>
</template>
<script>

import {useRoute} from 'vue-router';
import {reactive, toRefs} from 'vue';
export default {
  setup(){
    const route = useRoute();
    const pokemon = reactive({
      pokemon: null
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
    .then((res) => res.json())
    .then((data) => {
      pokemon.pokemon = data;
    })

    return {...toRefs(pokemon)};

}
}
</script>
