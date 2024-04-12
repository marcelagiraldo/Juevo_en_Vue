<template>
    <div class="container">
        <div class="fotos-row">
            <div class="foto" v-for="(imagen, index) in imagenes" :key="index">
                <img :src="imagen.src" :alt="imagen.alt">
            </div>
        </div>
        <input class="palabra" type="text" v-model="respuesta">
        <button class="boton-pasar-nivel" @click="verificarRespuesta">Enviar</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            categorias: {
                animal: [
                    { src: require('../assets/animal/gallina.webp'), alt: 'Gallina' },
                    { src: require('../assets/animal/perro.jpg'), alt: 'Perro' },
                    { src: require('../assets/animal/gato.jpg'), alt: 'Gato' },
                    { src: require('../assets/animal/panda.jpg'), alt: 'Panda' }
                ],
                // Agrega más categorías según sea necesario
            },
            categoriaSeleccionada: 'animal', // Cambia esto por la categoría inicial
            respuesta: ''
        };
    },
    computed: {
        imagenes() {
            return this.categorias[this.categoriaSeleccionada];
        }
    },
    methods: {
        verificarRespuesta() {
            // Agrega aquí la lógica para verificar la respuesta
        }
    }
};
</script>
<style>
    h1 {
        color:aliceblue;
        font-size: 60px;
    }
    .container {
        max-width: 800px; /* Ancho máximo del contenedor */
        margin: 0 auto; /* Centrar el contenedor horizontalmente */
        padding: 20px; /* Espaciado interno del contenedor */
    }

    .fotos-row {
        display: flex; /* Utiliza flexbox para organizar las imágenes */
        justify-content: space-around; /* Distribuye uniformemente las imágenes horizontalmente */
        margin-bottom: 20px; /* Espacio entre las filas de imágenes */
    }

    .foto {
        flex: 1; /* Las imágenes ocuparán el mismo ancho dentro de la fila */
        margin: 0 10px; /* Espacio horizontal entre las imágenes */
    }

    img {
        width: 70%; /* Las imágenes ocuparán todo el ancho disponible */
        display: block; /* Elimina el espacio adicional debajo de las imágenes */
    }
    .palabra{
        width: 60%;
        height: 40px;
        font-size: 40px;
        text-align: center;
    }
    .boton-pasar-nivel{
        width: 20%;
        height: 40px;
        margin-top: 10px;
    }
</style>
