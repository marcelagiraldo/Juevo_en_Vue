<template>
  <div class="container">
      <div class="titulo">
        <div class="monedas"><svg xmlns="http://www.w3.org/2000/svg" width="50px" height="50px" fill="currentColor" class="bi bi-currency-exchange" viewBox="0 0 16 16">
          <path color="rgb(221, 159, 25)" d="M0 5a5 5 0 0 0 4.027 4.905 6.5 6.5 0 0 1 .544-2.073C3.695 7.536 3.132 6.864 3 5.91h-.5v-.426h.466V5.05q-.001-.07.004-.135H2.5v-.427h.511C3.236 3.24 4.213 2.5 5.681 2.5c.316 0 .59.031.819.085v.733a3.5 3.5 0 0 0-.815-.082c-.919 0-1.538.466-1.734 1.252h1.917v.427h-1.98q-.004.07-.003.147v.422h1.983v.427H3.93c.118.602.468 1.03 1.005 1.229a6.5 6.5 0 0 1 4.97-3.113A5.002 5.002 0 0 0 0 5m16 5.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0m-7.75 1.322c.069.835.746 1.485 1.964 1.562V14h.54v-.62c1.259-.086 1.996-.74 1.996-1.69 0-.865-.563-1.31-1.57-1.54l-.426-.1V8.374c.54.06.884.347.966.745h.948c-.07-.804-.779-1.433-1.914-1.502V7h-.54v.629c-1.076.103-1.808.732-1.808 1.622 0 .787.544 1.288 1.45 1.493l.358.085v1.78c-.554-.08-.92-.376-1.003-.787zm1.96-1.895c-.532-.12-.82-.364-.82-.732 0-.41.311-.719.824-.809v1.54h-.005zm.622 1.044c.645.145.943.38.943.796 0 .474-.37.8-1.02.86v-1.674z"/>
        </svg>
        <h2 class="contenido-titulo">{{ cantidad_monedas }}</h2></div>
        <h1 class="contenido-titulo"><b>Nivel: <label for="">{{ nivel }}</label> </b></h1>
        <img v-if="vidauno" class="corazon" src="../assets/animal/corazon.png" alt="Corazon">
        <img v-if="vidados" class="corazon" src="../assets/animal/corazon.png" alt="Corazon">
        <img v-if="vidatres" class="corazon" src="../assets/animal/corazon.png" alt="Corazon">
      </div>
      <div class="contenido-principal">
        <div class="options">
          <button class="btn-option" @click="cambiarImagenes">
            <svg xmlns="http://www.w3.org/2000/svg" width="60px" height="60px" fill="currentColor" class="bi bi-arrow-clockwise" viewBox="0 0 16 16">
              <path color="rgb(31, 138, 165)" fill-rule="evenodd" d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2z"/>
              <path color="rgb(31, 138, 165)" d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466"/>
            </svg>
          </button>
          <button  class="btn-option" @click="funcionPista">
            <svg xmlns="http://www.w3.org/2000/svg" width="60px" height="60px" fill="currentColor" class="bi bi-lightbulb-fill" viewBox="0 0 16 16">
              <path color="rgb(31, 138, 165)" d="M2 6a6 6 0 1 1 10.174 4.31c-.203.196-.359.4-.453.619l-.762 1.769A.5.5 0 0 1 10.5 13h-5a.5.5 0 0 1-.46-.302l-.761-1.77a2 2 0 0 0-.453-.618A5.98 5.98 0 0 1 2 6m3 8.5a.5.5 0 0 1 .5-.5h5a.5.5 0 0 1 0 1l-.224.447a1 1 0 0 1-.894.553H6.618a1 1 0 0 1-.894-.553L5.5 15a.5.5 0 0 1-.5-.5"/>
            </svg>
          </button>
        </div>
        <div class="grid-container">
          <div class="grid-item" v-for="(image, index) in images" :key="index">
            <img class="imagenes" :src="image.url" @click="selectImage(index)" />
          </div>
          <input class="input-respuesta" v-model="userInput" type="text" placeholder="Ingresa tu respuesta" />
          <button class="boton" @click="checkAnswer">Comprobar</button>
        </div>
      </div>
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <p class="correcta">¡Respuesta correcta!</p>
        <svg xmlns="http://www.w3.org/2000/svg" width="50px" height="50px" fill="currentColor" class="bi bi-currency-exchange" viewBox="0 0 16 16">
          <path color="rgb(221, 159, 25)" d="M0 5a5 5 0 0 0 4.027 4.905 6.5 6.5 0 0 1 .544-2.073C3.695 7.536 3.132 6.864 3 5.91h-.5v-.426h.466V5.05q-.001-.07.004-.135H2.5v-.427h.511C3.236 3.24 4.213 2.5 5.681 2.5c.316 0 .59.031.819.085v.733a3.5 3.5 0 0 0-.815-.082c-.919 0-1.538.466-1.734 1.252h1.917v.427h-1.98q-.004.07-.003.147v.422h1.983v.427H3.93c.118.602.468 1.03 1.005 1.229a6.5 6.5 0 0 1 4.97-3.113A5.002 5.002 0 0 0 0 5m16 5.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0m-7.75 1.322c.069.835.746 1.485 1.964 1.562V14h.54v-.62c1.259-.086 1.996-.74 1.996-1.69 0-.865-.563-1.31-1.57-1.54l-.426-.1V8.374c.54.06.884.347.966.745h.948c-.07-.804-.779-1.433-1.914-1.502V7h-.54v.629c-1.076.103-1.808.732-1.808 1.622 0 .787.544 1.288 1.45 1.493l.358.085v1.78c-.554-.08-.92-.376-1.003-.787zm1.96-1.895c-.532-.12-.82-.364-.82-.732 0-.41.311-.719.824-.809v1.54h-.005zm.622 1.044c.645.145.943.38.943.796 0 .474-.37.8-1.02.86v-1.674z"/>
        </svg>
        <h1>+4</h1>
      </div>
    </div>
    <div class="modal" v-if="pista">
      <div class="modal-content">
        <p class="correcta">¡PISTA!</p>
        <div class="botones-pista"><button v-for="(item, index) in seleccionarPista" :key="index">{{ item }}</button></div>
      </div>
    </div>
    <div class="modal" v-if="cambiar">
      <div class="modal-content">
        <p class="correcta">Si aceptas pierdes 5 monedas</p>
        <svg xmlns="http://www.w3.org/2000/svg" width="50px" height="50px" fill="currentColor" class="bi bi-currency-exchange" viewBox="0 0 16 16">
          <path color="rgb(221, 159, 25)" d="M0 5a5 5 0 0 0 4.027 4.905 6.5 6.5 0 0 1 .544-2.073C3.695 7.536 3.132 6.864 3 5.91h-.5v-.426h.466V5.05q-.001-.07.004-.135H2.5v-.427h.511C3.236 3.24 4.213 2.5 5.681 2.5c.316 0 .59.031.819.085v.733a3.5 3.5 0 0 0-.815-.082c-.919 0-1.538.466-1.734 1.252h1.917v.427h-1.98q-.004.07-.003.147v.422h1.983v.427H3.93c.118.602.468 1.03 1.005 1.229a6.5 6.5 0 0 1 4.97-3.113A5.002 5.002 0 0 0 0 5m16 5.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0m-7.75 1.322c.069.835.746 1.485 1.964 1.562V14h.54v-.62c1.259-.086 1.996-.74 1.996-1.69 0-.865-.563-1.31-1.57-1.54l-.426-.1V8.374c.54.06.884.347.966.745h.948c-.07-.804-.779-1.433-1.914-1.502V7h-.54v.629c-1.076.103-1.808.732-1.808 1.622 0 .787.544 1.288 1.45 1.493l.358.085v1.78c-.554-.08-.92-.376-1.003-.787zm1.96-1.895c-.532-.12-.82-.364-.82-.732 0-.41.311-.719.824-.809v1.54h-.005zm.622 1.044c.645.145.943.38.943.796 0 .474-.37.8-1.02.86v-1.674z"/>
        </svg>
        <h1>-5</h1>
        <div class="botones-seleccion">
          <button @click="aceptar" class="check">
            <svg xmlns="http://www.w3.org/2000/svg" width="40px" height="40px" fill="currentColor" class="bi bi-check" viewBox="0 0 16 16">
              <path color="greenyellow" d="M10.97 4.97a.75.75 0 0 1 1.07 1.05l-3.99 4.99a.75.75 0 0 1-1.08.02L4.324 8.384a.75.75 0 1 1 1.06-1.06l2.094 2.093 3.473-4.425z"/>
            </svg>
          </button>
          <button @click="cancelar" class="x"><svg xmlns="http://www.w3.org/2000/svg" width="40px" height="40px" fill="currentColor" class="bi bi-x" viewBox="0 0 16 16">
            <path color="rgb(240, 85, 85)" d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708"/>
          </svg></button>
        </div>
      </div>
    </div>
    <div class="modal" v-if="showModalIncorrect">
      <div class="modal-content-incorrect">
        <p class="correcta">¡Respuesta incorrecta!</p>
        <div class="respuesta-incorrecta">
          <h1>-1</h1>
          <img  class="corazon-inc" src="../assets/animal/corazon.png" alt="Corazon">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';

export default {
  data() {
    const nivel = ref(1)
    const cantidad_monedas = ref(10)
    return {
      images: [],
      correctAnswer: 'respuesta_correcta',
      userInput: '',
      possibleAnswers: [],
      showModal: false,
      showModalIncorrect: false,
      vidauno: true,
      vidados: true,
      vidatres: true,
      vidas: 3,
      nivel,
      cantidad_monedas,
      cambiar:false,
      opcionCancelar:false,
      opcionAceptar:false,
      seleccionarPista:[],
      pista:false
    };
  },
  mounted() {
    this.fetchImagesByRandomCategory();
  },
  methods: {
    fetchImagesByRandomCategory() {
      const categoriesWithAnswers = [
        { category: 'nature', answers: ['naturaleza', 'bosque','bosques', 'paisaje', 'paisajes'] },
        { category: 'trees', answers: ['arboles','arbol'] },
        { category: 'architecture', answers: ['arquitectura','arquitecturas', 'edificio','edificios', 'ciudad','ciudades', 'diseño', 'diseños'] },
        { category: 'food', answers: ['comida', 'plato', 'platos'] },
        { category: 'kitchen', answers: ['cocina'] },
        { category: 'restaurant', answers: [ 'restaurante','restaurantes',] },
        { category: 'vegetables', answers: ['vegetales','vegetal'] },
        { category: 'fruits', answers: ['frutas','fruta'] },
        { category: 'childrens', answers: ['niños','niño'] },
        { category: 'numbers', answers: ['numeros','numero'] },
        { category: 'colors', answers: ['colores','color'] },
        { category: 'lenguage', answers: ['idiomas','idioma', 'letras','letra'] },
        { category: 'animals', answers: ['animales','animal'] },
        { category: 'dog', answers: ['perro','perros'] },
        { category: 'cat', answers: ['gato','gatos'] },
        { category: 'panda', answers: ['panda','pandas','osos','oso'] },
        { category: 'rabbit', answers: ['conejo','conejos'] },
        { category: 'pink', answers: ['rosado'] },
        { category: 'orange', answers: ['naranja','zapote'] },
        { category: 'blue', answers: ['azul'] },
        { category: 'sleep', answers: ['dormir'] },
        { category: 'happy', answers: ['feliz','felicidad'] },
        { category: 'sad', answers: ['triste','triste'] },
        { category: 'beer', answers: ['cerveza','cervezas'] },
        { category: 'bear', answers: ['oso','osos'] },
        { category: 'beard', answers: ['barba'] },
        { category: 'bird', answers: ['pajaro','pajaros'] },
      ];

      const randomCategoryData = categoriesWithAnswers[Math.floor(Math.random() * categoriesWithAnswers.length)];
      const randomCategory = randomCategoryData.category;
      this.possibleAnswers = randomCategoryData.answers; // Asignar las posibles respuestas a possibleAnswers

      const apiKey = 'FgqJgbUUg2T_mUe_evbPcYa7NvETLSwASmQCO_JtTCQ';
      const url = `https://api.unsplash.com/photos/random?count=4&query=${randomCategory}&client_id=${apiKey}`;
      axios.get(url)
        .then(response => {
          this.images = response.data.map(item => ({ url: item.urls.regular }));
          this.setCorrectAnswer();
        })
        .catch(error => {
          console.error('Error fetching images:', error);
        });
    },
    setCorrectAnswer() {
      // Seleccionar aleatoriamente una respuesta de la lista de posibles respuestas
      this.correctAnswer = this.possibleAnswers[Math.floor(Math.random() * this.possibleAnswers.length)];
      console.log(this.correctAnswer);
    },

    checkAnswer() {
      const userInputLowerCase = this.userInput.toLowerCase();
      if (this.possibleAnswers.some(answer => answer.toLowerCase() === userInputLowerCase)) {
        this.showCustomAlert(); // Llamar al método showCustomAlert en lugar de alert
        this.fetchImagesByRandomCategory();
        this.userInput = ''
        this.nivel += 1
        this.cantidad_monedas += 4
      } else {
        this.userInput = ''
        this.vidas -= 1
        if(this.vidas === 0){
          this.showCustomAlertIncorrect();
          this.vidas_corazon()
          this.fetchImagesByRandomCategory();
          this.vidauno = true
          this.vidados = true
          this.vidatres = true
        }else{
          this.showCustomAlertIncorrect();
          this.vidas_corazon()
        }


      }
    },
    showCustomAlert() {
      this.showModal = true;
      setTimeout(() => {
        this.showModal = false;
      }, 2000); // Cierra la alerta después de 2 segundos
    },
    showCustomAlertIncorrect() {
      this.showModalIncorrect = true;
      setTimeout(() => {
        this.showModalIncorrect = false;
      }, 2000); // Cierra la alerta después de 2 segundos
    },
    vidas_corazon(){
      if(this.vidas === 2){
        this.vidatres = false
      }else if (this.vidas ===1){
        this.vidados = false
      }else if(this.vidas === 0){
        this.vidauno = false
      }
    },
    aceptar(){
      this.cantidad_monedas -=5
      this.cambiar = false
      this.fetchImagesByRandomCategory();
    },
    cancelar(){
      this.cambiar = false
    },
    cambiarImagenes(){
      this.cambiar = true
    },
    funcionPista(){
      this.pista=true
      this.seleccionarPista = this.correctAnswer.split('')
      for (let i = 1; i < this.seleccionarPista.length; i+=2) {
          this.seleccionarPista[i] = '-';
      }
      console.log(this.seleccionarPista);
      setTimeout(() => {
        this.pista = false;
      }, 4000);
    }
  }
};
</script>

<style>
.botones-pista button{
  border-radius: 10px;
  width: 60px;
  height: 60px;
  margin-right: 7px;
  background-color: rgb(31, 138, 165);
  color: white;
}
.botones-seleccion .check{
  margin-right: 80px;
}
.botones-seleccion button{
  background-color: rgb(31, 138, 165);
  color: antiquewhite;
  border-radius: 40%;
  border-style: none;
}
.btn-option{
  background-color: transparent;
  border-style: none;
}
.contenido-principal{
  display: flex;
  margin: 0px;
  align-items: center;
  justify-content: center;
}
.options{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-right: 90px;
}
.options svg{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-bottom: 60px;
}
.container {
  height: 100%;
  width: 100%;
  margin: 0px 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
body,html{
  margin: 0px;
  padding: 0;
  width: 100%;
  background-color: rgb(50, 56, 59);
  color: antiquewhite;
}

.titulo {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin-bottom: 20px;
  margin-left: 150px;
}
.contenido-titulo{
  margin-left: 30px;
}
.monedas{
  display: grid;
  justify-content: center;
  align-items: center;

}
h1 {
  color: antiquewhite;
  font-size: 50px;
  margin: 0;
}

.corazon {
  width: 5%;
  height: auto;
  margin-left: 10px; /* Espacio entre el título y los corazones */
}

.imagenes {
  width: 250px;
  height: 250px;
}
.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 20px; /* Espacio entre las imágenes */
  justify-content: center; /* Centra el contenido horizontalmente */
  align-items: center; /* Centra el contenido verticalmente */
}
.input-respuesta, .boton{
  width: 100%;
  height: 40px;
  font-size: 20px;
  border-radius: 30px;
  text-align: center;
  line-height: 40px;
}
.boton{
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(31, 138, 165);
  color: antiquewhite;
}
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.modal-content {
  background-color: greenyellow;
  padding: 20px;
  width: 40%;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content h1{
  color: black;
}
.modal-content-incorrect {
  background-color: rgb(252, 131, 131);
  padding: 20px;
  border-radius: 10px;
}
.correcta{
  color: black;
}
.respuesta-incorrecta{
  display: flex;
  align-items: center;
  justify-content: center;
}
.corazon-inc{
  width: 80px;
}
.grid-item{
  border-style: double;
  border-color: rgb(31, 138, 165);
}
</style>

