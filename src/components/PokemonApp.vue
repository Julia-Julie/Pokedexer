<template>
  <div class="pokemons">
    <div class="container">
      <h2>{{ heading }}</h2>

      <div class="pokemons__content">
        <div class="pokemons__list">
          <div
            class="pokemon"
            v-for="(pokemon, index) in pokemons"
            v-bind:key="index"
          >
            <div class="pokemon__name">
              {{ pokemon.name }}
            </div>

            <div class="pokemon__types">
              Types:
              <!-- 'julia-27, orest-24'  -->
              <!-- [{name: "Julia", age: "27", village: "Birky"}, 
                      {name: "Orest", age: "24", village: "Vel Most"}] 
                        .map(el => ${el.name} - ${el.age}) -> ['julia-27', 'orest-24'].join(', ') -> 'julia-27, orest-24'
                      -->
              <!-- {}.(type = undefined).map()  -->
              <!-- {{ pokemon.types ? pokemon.types.map(el => el.type.name).join(', ') : ' ' }} -->

              <div class="pokemon__types--arr">
                {{ getTypes(pokemon) }}
              </div>
            </div>
          </div>
        </div>
        <div class="pokemons__chosen">Here should be a chosen Pokemon</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "PokemonApp",

  data: function () {
    return {
      heading: "Pokedex",
      pokemons: [],
    };
  },

  created() {
    this.getPokemons();
  },

  methods: {
    getPokemons() {
      axios("https://pokeapi.co/api/v2/pokemon?limit=12").then((response) => {
        // response = [{name: "", url=""}, {name: "", url=""}, {name: "", url=""}......]  --line 60
        response.data.results.forEach((pokemon) => {
          //(url) => axios(url) -> return response from axios (some data about single pokemon) --line 62
          this.makeRequest(pokemon.url).then((res) => {
            // then -> make some action with comming data{res.data} (we push data to general array "this.pokemons") --line 64
            this.pokemons.push(res.data);
          });
        });
      });
    },
    //function take one parameter{url} then meka axios request and return data from this request --line 71
    async makeRequest(url) {
      return await axios(url);
    },

    getTypes(pokemon) {
      return pokemon.types
        ? pokemon.types.map((el) => el.type.name).join(", ")
        : " ";
    },
  },
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
    @include items-align(grid, unset, space-between, stretch);
    grid-template-columns: repeat(3, 30%);
    flex-basis: 65%;
    border: 1px solid blue;
  }

  .pokemon {
    list-style: none;
    margin: 1rem;
    padding: 1rem 1.5rem;
    border: 1px dotted #ccc;

    &__name {
      color: rgb(12, 109, 64);
      font-size: 1.5rem;
      font-weight: bold;
      text-transform: capitalize;
      margin-bottom: 0.5rem;
    }

    &__types {
      color: rgb(3, 27, 16);
      font-size: 1rem;
      font-weight: 400;
      font-style: italic;
      margin-bottom: 0.3rem;

      &--arr{
        &:nth-child(odd){
          background-color: hotpink;
        }

         &:nth-child(even){
          background-color: rgb(223, 255, 105);
        }
      }
    }
  }

  &__chosen {
    flex-basis: 30%;
    border: 1px solid red;
  }
}
</style>