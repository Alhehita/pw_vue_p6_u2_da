<template>
  <div class="hijo">
    <img :src="image" alt="No se cargó el Pokémon">
    <p v-show="mostrarTexto">{{ hiddenText }}</p>
  </div>
</template>

<script>
export default {
  props: ['image', 'hiddenText', 'mostrarTexto']
}
</script>

<style>

.hijo img {
  width: 100px;
  height: 100px;
  object-fit: cover;
}

.hijo {
    display: block;
}

</style>
