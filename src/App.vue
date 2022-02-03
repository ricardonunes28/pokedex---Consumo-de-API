<template>
  <div id="app" class="has-background-link-dark">
    <div class="column is-full">
      <img src="./assets/pokémon.png" alt="" srcset="" />
      <div class="field has-addons">
        <div class="control" id="inputBusca">
          <input
            class="input is-rounded is-medium is-focused"
            type="text"
            placeholder="Pokémon"
            v-model="busca"
          />
        </div>
        <div class="control">
          <a
            class="button is-success is-rounded is-medium"
            id="buscaBtn"
            @click="buscar"
          >
            Buscar
          </a>
        </div>
      </div>
    </div>
    <div
      id="container"
      class="
        is-flex
        is-flex-wrap-wrap
        is-justify-content-space-between
        is-align-content-space-evenly
        is-align-items-center
      "
    >
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"; // fazer requisições http
import Pokemon from "./components/Pokemon.vue";
export default {
  name: "App",
  /**
   * Função data(dados) segue o padrão de retornar um objeto.
   *
   * Criando uma variavel para receber nossos pokemons da API
   */
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    /**
     * Pegar dados da API com o axios
     */
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegoou a lista de pokemons!"); // pegando os dados do pokemons com os objetos data e results, retorna os arrays com todos os pokemons
        this.pokemons = res.data.results;
        this.filteredPokemons = this.pokemons; // filtrando o array de pokemons para busca
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    /**
     *  Metodo de busca, fltrando os array de pokemons, foi declado em data filterpokemon como array, que é os arrays também de pokemon, a diferença que é nela que vamos fazer a filtragem e na outra não iremos mexer.
     *
     * função = Se busca for vazia ou um espaço em string vazio, apresentamos o this.pokemons a lista normalmente.
     * Se não this.filterdPokemon recebe this.pokemons.filter(filtrando pokemons pelo nome igual digitado em busca)
     */
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.busca
        );
      }
    },
  },
  // computed: {
  //   resultadoBusca: function() {
  //     if(this.busca =='' || this.busca == ' '){
  //       return this.pokemons;
  //     }else{
  //       return this.pokemons.filter(pokemon => pokemon.name == this.busca)
  //     }
  //   }
  // }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #090c0f;
}

#inputBusca{
  margin-left: 35%;
}

@media (max-width: 600px)
{
  #inputBusca{
  margin-left: 10%
}
}


#container {
  margin-left: 1%;
  margin-right: 1%;
  margin-top: 2%;
}
</style>
