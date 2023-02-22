<template>
  <section class="pokemon-container">
    <figure v-show="!showPokemon">
      <img :src=imageSrc class="hidden-pokemon" alt="pokemon">
    </figure>
    <figure v-if="showPokemon">
      <img id="original-image" :src=imageSrc class="fade-in" alt="pokemon">
      <canvas id="pixelated-image" class="fade-in"></canvas>
    </figure>
  </section>
</template>

<script>
export default {
  name: 'PokemonPicture',
  props: {
    pokemonId: {
      type: Number,
      required: true,
      default: 1
    },
    showPokemon: {
      type: Boolean,
      required: true,
      default: false
    }
  },
  computed: {
    imageSrc() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`
    }
  },
  updated() {
    const img = document.getElementById("original-image");
    const canvas = document.getElementById("pixelated-image");
    const ctx = canvas.getContext("2d");

    const pixelSize = 3; // Tamaño de los píxeles de la imagen pixelada

    canvas.width = img.width / pixelSize;
    canvas.height = img.height / pixelSize;

    ctx.imageSmoothingEnabled = false;
    ctx.drawImage(img, 0, 0, img.width, img.height, 0, 0, canvas.width, canvas.height);

    ctx.mozImageSmoothingEnabled = false;
    ctx.webkitImageSmoothingEnabled = false;
    ctx.msImageSmoothingEnabled = false;
    ctx.imageSmoothingEnabled = false;
  },
}
</script>