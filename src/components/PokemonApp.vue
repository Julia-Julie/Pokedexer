<template>
  <div class="pokemons">
    <div class="container">
      <h2 @click="testgetPokemons">{{ heading }}</h2>

      <div class="pokemons__content">
        <div class="pokemons__list">
          <div
            class="pokemon"
            v-for="(pokemon, index) in pokemons"
            v-bind:key="index"
          >
            <div class="pokemon__photo">
              <img v-bind:src="pokemon.sprites.back_default" />
            </div>

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

              <!-- <div class="pokemon__types--arr">
                {{ getTypes(pokemon) }}
              </div> -->

              <div
                v-for="(type, index) in pokemon.types"
                v-bind:key="index"
                class="pokemon__type"
                v-bind:class="type.type.name"
              >
                {{ type.type.name }}
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

    testgetPokemons() {
      axios("https://pokeapi.co/api/v2/pokemon?limit=12").then((response) => {
        // console.log(response.data.results)
        response.data.results.forEach((resultOne) => {
          console.log(resultOne, resultOne.url);
          this.makeRequest(resultOne.url).then((respId) =>
            console.log(
              `Id: ${respId.data.id}, Name: ${respId.data.name}, Height:${respId.data.height}`
            )
          );
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

%transition {
  transition: all ease-in-out 0.3s;
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
    // border: 1px dotted #ccc;
    box-shadow: 0px 0px 5px 2px #ccc;
    cursor: pointer;
    @extend %transition;

    &:hover {
      box-shadow: 0px 0px 7px 3px #bbb;
      transform: scale(1.1);

      .pokemon__name {
        color: rgb(139, 212, 69);
        @extend %transition;
        font-weight: bolder;
        text-shadow: 2px 2px 2px rgb(0, 0, 0);
      }
    }

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
    }

    &__type {
      display: inline-block;
      padding: 5px 10px;
      margin: 5px;
      font-weight: 300;
    }
  }

  &__chosen {
    flex-basis: 30%;
    border: 1px solid red;
  }
}

.grass {
      background-color: rgba(97, 192, 97, 0.61);
}

.poison {
  background-color: rgb(158, 99, 158);
}

.fire {
  background-color: rgb(221, 105, 90);
}
.flying {
  background-color: rgb(123, 190, 230);
}

.bug {
  background-color: rgb(211, 109, 177);
}

.water {
  background-color: rgb(43, 113, 170);
}
</style>