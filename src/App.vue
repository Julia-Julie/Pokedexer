<template>
  <div id="app">
    <div class="nav">
      <router-link to="/pokemons">Home link</router-link> |
    </div>
    <router-view />
  </div>
</template>



<script>
// import PokemonApp from './components/PokemonApp.vue'

export default {
  name: 'App',
  components: {
    // PokemonApp
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
