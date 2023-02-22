<template>
  <section>
    <ul>
      <li v-for="pokemon in pokemons" :key="pokemon.id" @click="$emit('selection', pokemon.id)" class="nes-btn">
        {{ pokemon.name }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: 'PokemonOptions',
  props: {
    pokemons: {
      type: Array,
      required: true,
    }
  },
}
</script>