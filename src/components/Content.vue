<template>
  <div class="container">
    <div class="titulo">
      <div class="monedas"><svg xmlns="http://www.w3.org/2000/svg" width="50px" height="50px" fill="currentColor" class="bi bi-currency-exchange" viewBox="0 0 16 16">
        <path color="rgb(221, 159, 25)" d="M0 5a5 5 0 0 0 4.027 4.905 6.5 6.5 0 0 1 .544-2.073C3.695 7.536 3.132 6.864 3 5.91h-.5v-.426h.466V5.05q-.001-.07.004-.135H2.5v-.427h.511C3.236 3.24 4.213 2.5 5.681 2.5c.316 0 .59.031.819.085v.733a3.5 3.5 0 0 0-.815-.082c-.919 0-1.538.466-1.734 1.252h1.917v.427h-1.98q-.004.07-.003.147v.422h1.983v.427H3.93c.118.602.468 1.03 1.005 1.229a6.5 6.5 0 0 1 4.97-3.113A5.002 5.002 0 0 0 0 5m16 5.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0m-7.75 1.322c.069.835.746 1.485 1.964 1.562V14h.54v-.62c1.259-.086 1.996-.74 1.996-1.69 0-.865-.563-1.31-1.57-1.54l-.426-.1V8.374c.54.06.884.347.966.745h.948c-.07-.804-.779-1.433-1.914-1.502V7h-.54v.629c-1.076.103-1.808.732-1.808 1.622 0 .787.544 1.288 1.45 1.493l.358.085v1.78c-.554-.08-.92-.376-1.003-.787zm1.96-1.895c-.532-.12-.82-.364-.82-.732 0-.41.311-.719.824-.809v1.54h-.005zm.622 1.044c.645.145.943.38.943.796 0 .474-.37.8-1.02.86v-1.674z"/>
      </svg>
      <h2 class="contenido-titulo">{{ cantidad_monedas }}</h2></div>
      <h1 class="contenido-titulo"><b>Nivel: <label for="">{{ nivel }}</label> </b></h1>
      <img v-if="vidauno" class="corazon" src="../assets/animal/corazon.png" alt="Gallina">
      <img v-if="vidados" class="corazon" src="../assets/animal/corazon.png" alt="Gallina">
      <img v-if="vidatres" class="corazon" src="../assets/animal/corazon.png" alt="Gallina">
    </div>
    <div class="grid-container">
      <div class="grid-item" v-for="(image, index) in images" :key="index">
        <img class="imagenes" :src="image.url" @click="selectImage(index)" />
      </div>
      <input class="input-respuesta" v-model="userInput" type="text" placeholder="Ingresa tu respuesta" />
      <button class="boton" @click="checkAnswer">Comprobar</button>
    </div>
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <p class="correcta">¡Respuesta correcta!</p>
        <!-- Aquí puedes agregar contenido personalizado, como botones, texto adicional, etc. -->
      </div>
    </div>
    <div class="modal" v-if="showModalIncorrect">
      <div class="modal-content-incorrect">
        <p class="correcta">¡Respuesta incorrecta!</p>
        <!-- Aquí puedes agregar contenido personalizado, como botones, texto adicional, etc. -->
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
      possibleAnswers: [], // Definir possibleAnswers como una propiedad de datos
      showModal: false, // Agregar showModal al objeto data
      showModalIncorrect: false,
      vidauno: true,
      vidados: true,
      vidatres: true,
      vidas: 3,
      nivel,
      cantidad_monedas
    };
  },
  mounted() {
    this.fetchImagesByRandomCategory();
  },
  methods: {
    fetchImages() {
      const apiKey = 'vfEdfktHNI5qqI5LoUnWlFlG14Z5T8WjrzPUuzWtz0Q';
      axios.get(`https://api.unsplash.com/photos/random?count=4&client_id=${apiKey}`)
        .then(response => {
          this.images = response.data.map(item => ({ url: item.urls.regular }));
        })
        .catch(error => {
          console.error('Error fetching images:', error);
        });
    },
    fetchImagesByRandomCategory() {
      const categoriesWithAnswers = [
        { category: 'nature', answers: ['naturaleza', 'bosque', 'árboles', 'paisaje'] },
        { category: 'architecture', answers: ['arquitectura', 'edificio', 'ciudad', 'diseño'] },
        { category: 'food', answers: ['comida', 'cocina', 'restaurante', 'plato'] },
        // Agrega más categorías con sus posibles respuestas aquí
      ];

      const randomCategoryData = categoriesWithAnswers[Math.floor(Math.random() * categoriesWithAnswers.length)];
      const randomCategory = randomCategoryData.category;
      this.possibleAnswers = randomCategoryData.answers; // Asignar las posibles respuestas a possibleAnswers

      const apiKey = 'GTkoGc3gCtyjlkbqcfwQiqgT2ox8xWIw1rWen5YoR9k';
      const url = `https://api.unsplash.com/photos/random?count=4&query=${randomCategory}&client_id=${apiKey}`;
      axios.get(url)
        .then(response => {
          this.images = response.data.map(item => ({ url: item.urls.regular }));
          this.setCorrectAnswer(); // Establecer la respuesta correcta
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
    }
  }
};
</script>

<style scoped>
.container {
  height: 100%;
  width: 100%;
  margin: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
body,html{
  margin: 0;
  padding: 0;
  width: 100%;
}

.titulo {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin-bottom: 20px;
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
  color: black;
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
  background-color: aqua;
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
  background-color: rgb(221, 159, 25);
  padding: 20px;
  width: 40%;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content-incorrect {
  background-color: red;
  padding: 20px;
  border-radius: 10px;
}
.correcta{
  color: black;
}
</style>

