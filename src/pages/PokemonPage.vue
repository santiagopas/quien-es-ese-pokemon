<template>
  <div class="wrapper">
    <h2 v-if="!pokemon">Cargando...</h2>
    <header v-show="pokemon">
      <i class="nes-pokeball"></i>
      <div class="nes-balloon from-left">
        <h1>Quién es ese Pokemon?!</h1>
      </div>
      <section class="puntos">
        <p class="nes-badge ">
          <span class="is-success">Puntos: {{ puntos }}</span>
        </p>
      </section>
    </header>

    <main v-if="pokemon">
      <PokemonPicture :pokemonId='pokemon.id' :showPokemon="showPokemon" />
      <PokemonOptions v-if="!showAnwer" :pokemons="pokemonArr" @selection="checkAnswer" />
      <template v-if="showAnwer">
        <section class="answer fade-in">
          <div class="nes-balloon from-right">
            <h3> <i class="nes-ash"></i> {{ message }}</h3>
          </div>
          <button class="nes-btn" @click="newGame">Intentalo nuevamente</button>
        </section>
      </template>
    </main>

  </div>
</template>

<script>
// Components
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
// Helpers
import getPokemonOptions from '@/helpers/getPokemonOptions.js'
import { isClass } from '@babel/types'

export default {
  name: 'PokemonPage',
  components: {
    PokemonPicture,
    PokemonOptions,
    isClass
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnwer: false,
      message: '',
      puntos: 0,
    }
  },
  methods: {
    async mixPokemonsArray() {
      this.pokemonArr = await getPokemonOptions();
      const rndInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(pokemonId) {
      this.showPokemon = true
      this.showAnwer = true
      if (pokemonId === this.pokemon.id) {
        this.message = `¡Correcto!, era ${this.pokemon.name}`
        this.aumentarPuntaje()
      } else {
        this.message = `Ups!, era ${this.pokemon.name}`
        this.restarPuntos()
        finDelJuego()
      }
    },
    aumentarPuntaje() {
      let puntos = this.puntos
      this.puntos = puntos + 1
    },
    restarPuntos() {
      let puntos = this.puntos
      this.puntos = puntos - 1
      if (this.puntos < 0) {
        this.puntos = 0

      }
    },
    newGame() {
      this.showPokemon = false
      this.showAnwer = false
      this.pokemonArr = []
      this.mixPokemonsArray()
      
      this.pokemon = null
    },
    finDelJuego() {

      if (this.puntos === 0) {
        this.message = `Fin del juego`
        this.showAnwer = true

      }

    }
  },
  mounted() {
    this.mixPokemonsArray()
  },
}
</script>
