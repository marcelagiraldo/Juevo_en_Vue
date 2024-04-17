<script setup>
import Content from './components/Content.vue';
</script>

<template>
  <div class="container">
    <Content />
    <div class="bienvenida">
      <h1>Bienvenido </h1>
      <div class="instrucciones">
        <h3>El Juego 4 Fotos y 1 palabra esta diseñado para que los jugadores puedan practicar el
          reconocimiento de imagenes. <br><br>
          <b>**************************Instrucciones**************************
            <ul>
              <li>Observa cada una de las imagenes e ingresa una palabra que pueda definir las imagenes</li>
              <li>Si su respuesta es incorrecta tendrá tres oportunidades para ganar y pasar al siguiente nivel,
                de lo contrario perderá puntos y volverá a empezar el juego</li>
              <li>Por cada respuesta correcta sube de nivel y va ganando puntos</li>
            </ul>
          </b>
        </h3>
      </div>
      <br>
      <router-link to="/juego">
        <svg xmlns="http://www.w3.org/2000/svg" width="120px" height="120px" fill="currentColor" class="bi bi-play-fill" viewBox="0 0 16 16">
          <path d="m11.596 8.697-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393"/>
        </svg>
      </router-link>
    </div>
    <main>

    </main>
  </div>

</template>

<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 25px;
  }
  body,html{
    margin: 0;
    padding: 0;
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .bienvenida h1{
    color: aqua;
    display: flex;
    justify-content: center;
    margin-bottom: 40px;
  }
.bienvenida {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.instrucciones {
  width: 80%;
  margin-top: 20px; /* Ajusta el margen superior según sea necesario */
}

.instrucciones h3 {
  font-weight: normal; /* Quita el peso de la fuente negrita */
}

.instrucciones ul {
  padding: 0; /* Elimina el relleno predeterminado de la lista */
  margin: 0; /* Elimina el margen predeterminado de la lista */
  text-align: left; /* Alinea el texto de la lista a la izquierda */
}
.btn-empezar{
  background-color: aqua;
  width: 300px;
  font-size: 30px;
  border-radius: 30px;
}
</style>
