<template>
  <div class="container">
    <div class="titulo">
      <h1>4 Fotos 1 Palabra</h1>
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

export default {
  data() {
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
      vidas: 3
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
      } else {
        this.userInput = ''
        this.vidas -= 1
        this.showCustomAlertIncorrect();
        this.vidas_corazon()
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

.titulo {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin-bottom: 20px; /* Agregamos un margen inferior para separar el título de las imágenes */
}

h1 {
  color: aliceblue;
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
  padding: 20px;
  border-radius: 10px;
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

