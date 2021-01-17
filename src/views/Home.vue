<template>
  <div class="grid justify-center">
    <div>
      <input v-model="text" class="border rounded p-2 focus:outline-none" type="text" placeholder="Fill Pokemon Name" >
    </div>
    <div class="flex flex-wrap justify-center">
        <div v-for="(pokemon,idx) in filterPokemons" :key="idx" class="flex justify-center text-blue-900 pl-2">
          <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
            {{ pokemon.name }}
          </router-link>
        </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import {reactive,toRefs, computed} from "vue";
export default {
  setup(){
    const state = reactive({
      pokemons:[],
      filterPokemons:computed(() => updatePokemons()),
      text : '',      
      urlIdLookup:{}
    });
    
    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
    .then((res) => res.json())
    .then((data) => {
      console.log(data);
      state.pokemons = data.results;
      state.urlIdLookup = data.results.reduce((acc, cur, idx)=> 
           acc = {...acc, [cur.name]:idx+1 }
      ,{})
      console.log(state.urlIdLookup);
    });

    function updatePokemons(){
      if(!state.text){
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      )
    }

    return { ...toRefs(state) };
  }
}
</script>
