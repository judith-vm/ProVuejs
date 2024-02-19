<template>
  <div class="container">
      <!-- Si no se esta mostrando cuestionario -->
    <h1 v-if="!mostrarCuestionario" class="col-12 col-sm-6 col-md-8">Cuestionario Cultura General</h1>
    <img v-if="!mostrarCuestionario" class="col-12 col-sm-6 col-md-12" src="./mundo.jpg" alt="Imagen plano del mundo" />
    <div v-if="!mostrarCuestionario">
         <!-- Si damos clic mostrarCuestionario se vuelve true -->
      <button class="btn btn-success btn-lg  btn-sm" @click="mostrarCuestionario = true">Iniciar</button>
    </div>

    <div v-else>
          <!-- Sino se estan mostrando resultados significa que estamos en el cuestionario -->
      <div v-if="!mostrarResultados" >
            <!-- Iteracion para mostrar las preguntas -->
        <div v-for="(pregunta, index) in preguntas" :key="index">
          <p class="preguntaS">{{ pregunta.texto }}</p>
            <div v-for="(opcion, i) in pregunta.opciones" :key="i">
              <label>
                <input type="radio" :value="opcion" v-model="respuestas[index]">
                {{ opcion }}
              </label>
            </div>
        </div>
              <!-- Cuando terminamos de contestar se llama a la funcion calcularPuntaje -->
        <button class="btn btn-success btn-lg  btn-sm" @click="calcularPuntaje">Finalizar</button>
      </div>
      
      <div v-else>
            <!-- Mostrar resultados -->
        <h3 class="Resultado">Resultados</h3><br>
        <p class="Puntaje">Tu puntaje es: {{ puntaje }}/5</p>
        <h3>Respuestas</h3>
        <div v-for="(pregunta, index) in preguntas" :key="index">
          <p class="preguntaS">{{ pregunta.texto }}</p>
              <!-- Mostrar respuestas seleccionadas y validar -->
          <div class="col-12 col-sm-6 col-md-12">
            <div v-for="(opcion, i) in pregunta.opciones" :key="i" :class="{ 'respuesta-seleccionada': esRespuestaSeleccionada(index, opcion), 'respuesta-correcta': esRespuestaCorrecta(index, opcion), 'respuesta-incorrecta': esRespuestaIncorrecta(index, opcion) }">
              {{ opcion }}
            </div>
            </div>
        </div>
          <!-- Al repetir cuestionario se resetean los valores -->
        <button class="btn btn-success btn-lg  btn-sm" @click="repetirCuestionario">Repetir</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      preguntas: [
        {
          texto: '¿Cuál es el océano más grande del mundo?',
          opciones: ['Océano Pacífico', 'Océano Atlántico', 'Océano Índico', 'Océano Ártico'],
          respuestaCorrecta: 'Océano Pacífico'
        },
        {
          texto: '¿Quién pintó la Mona Lisa?',
          opciones: ['Vincent van Gogh', 'Leonardo da Vinci', 'Pablo Picasso', 'Frida Kahlo'],
          respuestaCorrecta: 'Leonardo da Vinci'
        },
        {
          texto: '¿Cuál es el planeta más grande del sistema solar?',
          opciones: ['Tierra', 'Saturno', 'Marte', 'Júpiter'],
          respuestaCorrecta: 'Júpiter'
        },
        {
          texto: 'Proceso mediante el cual las plantas obtienen alimento',
          opciones: ['Transpiración', 'Respiración celular', 'Fotosíntesis', 'Nutrición autótrofa'],
          respuestaCorrecta: 'Fotosíntesis'
        },
        {
          texto: '¿Cuántos lados tiene un hexágono?',
          opciones: ['5', '10', '6', '8'],
          respuestaCorrecta: '6'
        },
      ],

      //Para guardar las respuestas
      respuestas: [],
      // Banderas de control
      mostrarCuestionario: false,
      mostrarResultados: false,
      puntaje: 0
    };
  },
  methods: {
    calcularPuntaje() {
      this.puntaje = 0;
      for (let i = 0; i < this.preguntas.length; i++) {
        if (this.respuestas[i] === this.preguntas[i].respuestaCorrecta) {
          this.puntaje++;
        }
      }
      this.mostrarResultados = true;
    },
    repetirCuestionario() {
      this.respuestas = [];
      this.mostrarCuestionario = false;
      this.mostrarResultados = false;
      this.puntaje = 0;
    },
    esRespuestaSeleccionada(index, opcion) {
      return this.respuestas[index] === opcion;
    },
    esRespuestaCorrecta(index, opcion) {
      return this.respuestas[index] === opcion && opcion === this.preguntas[index].respuestaCorrecta;
    },
    esRespuestaIncorrecta(index, opcion) {
      return this.respuestas[index] === opcion && opcion !== this.preguntas[index].respuestaCorrecta;
    }
  }
};
</script>

<style scoped>
.container {
  font-family: 'Verdana';
  text-align: center;
  margin-top: 20px;
}
.preguntaS{
  font-weight: bold;
}
.respuesta-seleccionada {
  font-weight: bold;
}
.respuesta-correcta {
  background: #05E35D;
  display: inline-block;
  text-align: center;
  padding: 5px 5px;
  border-radius: 15px 15px 15px 15px;
}
.respuesta-incorrecta {
  background: #FB3D30;
  display: inline-block;
  text-align: center;
  padding: 5px 5px;
  border-radius: 15px 15px 15px 15px;
}
button{
    margin-top: 15px;
    margin-bottom: 15px;
    font-family: 'Verdana';
    font-weight: bold;
    background: #28a745;
    color: #fff;
    border: 20px;
    padding: 10px 8px;
    border-radius: 20px 20px 20px 20px;
}

.Resultado{
    background: #AAEFFA;
    display: inline-block;
    text-align: center;
    font-weight: bold;
    padding: 5px 5px;
    border-radius: 15px 15px 15px 15px;
}
h1{
  color: #fff;
  display: inline-block;
  background: #C89F6E;
  border-radius: 15px 15px 15px 15px;
}
img{
  max-height: 500px; 
  width: auto; 
}
.Puntaje{
    background: #F9F2A1;
    display: inline-block;
    text-align: center;
    font-weight: bold;
    padding: 5px 5px;
    border-radius: 15px 15px 15px 15px;
}


</style>
