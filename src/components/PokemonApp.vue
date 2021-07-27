<template>
  <div class="pokemons">
    <div class="container">
      <h2>{{ heading }}</h2>

      <div class="pokemons__content">
        <ul class="pokemons__list">
          <li
            class="pokemons__item"
            v-for="(pokemon, index) in pokemons"
            v-bind:key="index"
          >
            {{ pokemon.name }}
            {{ pokemon.type }}
          </li>
        </ul>

        <div class="pokemons__chosen">Here should be a chosen Pokemon</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonApp",
  // props: {
  //   msg: String
  // },

  data: function () {
    return {
      heading: "Pokedex",
      pokemons: [
        { name: "pikachu", type: "fire" },

        { name: "chobakabra", type: "fire" },

        { name: "spider", type: "iron" },

        { name: "clablah", type: "stone" },
      ],
    };
  },

  // methods: {
  // getCharachers(id) {
  //   fetch(`https://swapi.dev/api/people/${id}`, {
  //     method: "Get",
  //   })
  //     .then((response) => response.json())
  //     .then((json) => (this.character = json));
  // },
};
</script>

<style scoped lang="scss">
*,
::after,
::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  margin: 0 10%;
}

@mixin items-align($display, $row, $horizontal, $vertical) {
  display: $display;
  flex-direction: $row;
  justify-content: $horizontal;
  align-items: $vertical;
}

.pokemons {
  &__content {
    @include items-align(flex, row, space-between, center);
  }

  &__list {
    @include items-align(grid, unset, space-between, center);
    grid-template-columns: repeat(3, 30%);
    flex-basis: 60%;
    border: 1px solid blue;
  }

  &__chosen {
    flex-basis: 35%;
    border: 1px solid red;
  }
}
</style>
