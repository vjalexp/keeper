<template>

  <div class="grid grid-cols-4 m-10 border-yellow-400 border-2">
    <div class="col-span-3 bg-yellow-400">
      <div class="m-5 text-2xl text-yellow-700">
        <router-link :to="`/`">Pokemon Keeper</router-link>
      </div>
    </div>
  
  <router-view/>
  
  </div>

</template>

<style>

</style>
