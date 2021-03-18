<template>

  <div class="bg-yellow-400">
    <div class="m-4 p-2 rounded-lg ring-4 ring-yellow-500 text-yellow-700 text-center cursor-pointer">
      <router-link :to="`/starred`">Starred</router-link>
    </div>
  </div>

  <div class="m-4">
    <input class="w-full p-5 border-yellow-500 border-2 focus:outline-none" type="text" placeholder="Type..." v-model="text" />
  </div>

  <div class="col-span-3 m-4 justify-start">
    <div class="ml-2 mb-1 text-yellow-500" v-for="pokemon in filteredPokemons" :key="pokemon">
      <div class="p-2 bg-yellow-100">
        <input
          type="checkbox"
          name="pokemon"
          :id="pokemon"
          :value="pokemon"
          v-model="checked"
        />
        <label :for="pokemon">{{ pokemon.name }}</label>
      </div>
    </div>
  </div>

</template>

<script>
import { reactive, toRefs, computed } from "vue";
export default {
  mounted() {
    this.checked = JSON.parse(localStorage.getItem("checked")) || []
  },
  watch: {
    checked(newValue) {
      localStorage.setItem("checked", JSON.stringify(newValue));
    }
  },
  setup() {
    const state = reactive({
      checked: [],
      pokemons: [],
      urlIdLookup: {},
      text: "",
      filteredPokemons: computed(()=> updatePokemon())   
    })

    function updatePokemon() {
      if(!state.text) {
        return []
      }
      
      return state.pokemons.filter((pokemon)=>
        pokemon.name.includes(state.text)
      )
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((acc, cur, idx)=> 
           acc = {...acc, [cur.name]:idx+1 }
        ,{})
    })
    
    return { ...toRefs(state) };
  }
}
</script>