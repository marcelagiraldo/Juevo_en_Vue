<script setup>
  import Titulo from "./components/Titulo.vue";
  import Contenido from "./components/Contenido.vue"
</script>

<template>
  <div class="container">
    <header>
      <Titulo></Titulo>
    </header>
    <main>
      <Contenido></Contenido>
    </main>
  </div>

</template>

<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center; /* Centra los elementos horizontalmente */
    justify-content: center; /* Centra los elementos verticalmente */
  }
  body,html{
    margin: 0;
    padding: 0;
    width: 100%;
  }
</style>
