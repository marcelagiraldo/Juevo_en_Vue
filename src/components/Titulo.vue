<script>

</script>

<template>
    <div class="titulo">
        <h1>4 Fotos 1 Palabra</h1>
        <img class="corazon" src="../assets/animal/corazon.png" alt="Gallina">
        <img class="corazon" src="../assets/animal/corazon.png" alt="Gallina">
        <img class="corazon" src="../assets/animal/corazon.png" alt="Gallina">

    </div>

</template>
<style>
    body, html {
        margin: 0;
        padding: 0;
        height: 100%;
    }

    .titulo {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        text-align: center; /* Centrar el texto horizontalmente */
    }

    h1 {
        color: aliceblue;
        font-size: 20px;
        margin: 0;
    }

    .corazon {
        width: 5%;
        height: auto;
        margin-left: 10px; /* Espacio entre el título y los corazones */
    }
</style>
