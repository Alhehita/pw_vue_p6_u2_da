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
.hijo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hijo img {
  width: 100px;
  height: 100px;
  object-fit: cover;
}

.hijo p {
  margin-top: 5px;
  text-align: center;
}
</style>
