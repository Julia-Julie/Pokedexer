<template>
  <div class="home">
    {{ title }}
    <PokemonApp/>
  </div>
</template>

<script>
import PokemonApp from '../components/PokemonApp.vue'

export default {
  name: "Home",

  data(){
    return {
      title: 'Home title'
    }
  },

  components:{
    PokemonApp
  }
}
</script>
