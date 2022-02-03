<template>
  <div id="pokemon" >
    <!--Variaveis sendo utilizadas aqui com o data-binding  -->

    <div class="card">
      <div class="card-image">
        <figure>
          <img id="imgPoke" :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-5">{{ upper(name) }}</p>
            <p class="subtitle is-6">{{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <button class="button is-medium is-fullwidth" @click="mudarSprite">
            Mudar Sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  /**
   * A função nesse componente Pokemon , é para fazer a chamada de alguns aspectos do nossos pokemons , extraidos da API.
   */
  created: function () {
    // dentro do metodo created vai ser chamado para cada pokemon , get com a url (conteudo da props)
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },
  /**
   * data() com o object pokemon
   */
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        // Um dado só será reativo se ele for declarado dentro da função data! info - (exemplo- this.pokemon.type)
        type: "",
        front: "",
        back: "",
      },
    };
  },
  /**
   *  Recebendo props, referente ao nome e url dos pokemons na API.
   *
   */
  props: {
    num: Number,
    name: String,
    url: String,
  },
  methods: {
    upper: function (value) {
      // adicionando na variavel name que trás o nome do pokemon a letra inical como maiuscala em todas.
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
    /**
     *
     * Função para o Button, invertor irmagem de pokemon front e back (frente e costas)
     *
     * isFront está por padrão como true em data()
     */
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>
<style scoped>
#pokemon {
  margin-top: 2%;
}

#imgPoke{
  height: 20vh;
}


</style>