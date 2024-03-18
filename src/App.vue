<template>
  <div id="app">
    <PaginaInicial />
  </div>
</template>

<script>
import PaginaInicial from "@/view/PaginaInicial.vue";

export default {
  name: "App",
  components: {
    PaginaInicial,
  },
};
</script>

<style>
body {
  margin: 0px;
  padding: 0px;
}
/* Estilos globais do aplicativo */
#app {
  font-family: Arial, sans-serif;
  background-image: url("./assets/images/wave.svg"); /* Caminho para a imagem de plano de fundo */
  background-size: cover;
  height: 50vh;
 
}
header {
  background-color: #007bff;
  color: white;
  padding: 20px;
}
</style>
