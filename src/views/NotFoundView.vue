<script setup>
import { RouterLink } from "vue-router";
</script>

<template>
  <div class="wrapper">
    <h1 class="title">Erro: 404</h1>
    <p>Página não encontrada.</p>
  </div>
</template>

<script>
export default {
  name: "NotFoundView",
};
</script>

<style scoped>
.wrapper {
  max-width: 50rem;
  padding: 0 1rem;
  margin: 3.75rem auto;
  font-size: 0.9rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.title {
  font-family: "Spectral", Georgia;
  font-size: 3rem;
  position: relative;
  line-height: 1;
}

.title:after {
  content: "";
  display: block;
  width: 1.5rem;
  height: 1.5rem;
  background: #42b883;
  position: absolute;
  bottom: 5px;
  left: -5px;
  border-radius: 0.2rem;
  z-index: -1;
}
</style>
