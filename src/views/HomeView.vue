<template>
  <div>
    <v-col color="white" variant="outlined" class="text-center container" cols="12">
      <h1 class="text" id="title">Lista de cursos</h1>
      <h3 class="text">Aquí encontrarás los cursos disponibles.</h3>
    </v-col>

    <div class="grid-courses justify-center">
      <div class="courses" v-for="(curso, key, index) in cursos" :key="index">
        <center><img id="image" alt="poster" width="300" height="250" :src="curso.img" /></center>
        <p class="box">
          <strong>
            <center>{{ curso.nombre }}</center>
          </strong>
        </p>
        <h3>
          <center>Información</center>
        </h3>
        <p class="box">
          <center>Costo: {{ curso.costo }}</center>
        </p>
        <p class="box">
          <center>Duración: {{ curso.duracion }}</center>
        </p>
        <p class="box">
          <center>Cupos: {{ curso.cupos }}</center>
        </p>
        <p class="box">
          <center>Completado: {{ curso.completado }}</center>
        </p>
        <p class="box">
          <center>Fecha de registro: {{ curso.fecha_registro }}</center>
        </p>
        <p class="box">
          <center>Descripción: {{ curso.descripcion }}</center>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'HomeView',
  data() {
    return {

    }
  },
  computed: {
    ...mapState(['cursos'])
  }
}
</script>

<style scoped>
div {
  margin-top: 3em;
}

#title {
  font-size: 3em;
}

.grid-courses {
  margin-left: 10%;
  margin-right: 10%;
  width: 80%;
  display: grid;
  gap: 1px;
  grid-template-columns: repeat(3, 1fr);
}

.courses {
  background-color: white;
  display: inline-block;
  margin: 10px;
  border-radius: 25px;
  border: 2px;
  padding: 2%;
}

.text {
  margin-top: 0.5em;
  margin-bottom: 0.5em;
}

.container {
  width: 30%;

}</style>
