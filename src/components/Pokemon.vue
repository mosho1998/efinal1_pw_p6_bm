<template>
  <div class="pokemon">
    <img :src="img" alt="Pokemon no disponible" />
    <p>{{ texto }}</p>
  </div>
</template>

<script>
export default {
  props: {
    img: String,
    texto: String,
  },
};
</script>

<style>
img {
  margin: 10px;
  width: 150px;
  height: 150px;
}
</style>