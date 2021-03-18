<template>

  <div class="bg-yellow-400">
    <div class="m-4 p-2 rounded-lg ring-4 ring-yellow-500 bg-yellow-700 text-yellow-500 text-center cursor-pointer">
      <router-link :to="`/starred`">Starred</router-link>
    </div>
  </div>
  
  <div class="m-4">
    &nbsp;
  </div>

  <draggable class="col-span-3 m-4 justify-start" :list="all" @change="saveOrder">
    <div class="ml-2 mb-1 text-yellow-500 cursor-move" v-for="pokemon in all" :key="pokemon">
      <div class="p-2 bg-yellow-100">
        <input
          type="checkbox"
          name="pokemon"
          :id="pokemon"
          :value="pokemon"
          v-model="checked"
        />
        <label :for="pokemon">{{ pokemon.name }}</label>
        <span class="ml-10">&udarr;</span>
      </div>
    </div>
  </draggable>

</template>

<script>
import { reactive, toRefs } from "vue";
import { VueDraggableNext } from 'vue-draggable-next'

export default {
  components: {
    draggable: VueDraggableNext,
  },
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
      all: JSON.parse(localStorage.getItem("checked")),
      checked: []   
    })

    function saveOrder(event) {
        console.log(event)
    }
    
    return { ...toRefs(state), saveOrder };
  }
}
</script>