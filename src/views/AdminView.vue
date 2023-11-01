<template>
  <div class="heroesTable">
    <v-col color="white" variant="outlined" class="text-center container" cols="12">
      <h1 class="text" id="title">Administración</h1>
    </v-col>

    <div class="addButton">
      <v-row justify="center">
        <v-btn color="primary" @click="dialog = true;" dark>
          Agregar curso
        </v-btn>
      </v-row>
    </div>

    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-center">
              Curso
            </th>
            <th class="text-center">
              Cupos
            </th>
            <th class="text-center">
              Inscritos
            </th>
            <th class="text-center">
              Duración
            </th>
            <th class="text-center">
              Costo
            </th>
            <th class="text-center">
              Terminado
            </th>
            <th class="text-center">
              Fecha
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="curso in cursos" :key="curso.id">
            <td>{{ curso.nombre }}</td>
            <td>{{ curso.cupos }}</td>
            <td>{{ curso.inscritos }}</td>
            <td>{{ curso.duracion }}</td>
            <td>${{ curso.costo }}</td>
            <td>{{ curso.completado }}</td>
            <td>{{ curso.fecha_registro }}</td>
            <td>
              <v-btn @click="setIDCourseEdit(curso.id); dialog2 = true; showID()" color="success">
                <v-icon left>
                  mdi-pencil
                </v-icon>
                Editar
              </v-btn>
            </td>
            <td>
              <v-btn @click="deleteCourse(curso.id)" color="red">
                <v-icon left>
                  mdi-delete
                </v-icon>
                Eliminar
              </v-btn>
            </td>

          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <div class="text-center">
      <v-dialog v-model="dialog" width="auto">
        <v-card width="400px" class="mx-auto mt-5">
          <v-card-title primary-title>
            <h1 class="mx-auto">Nuevo curso</h1>
          </v-card-title>
          <v-card-text>
            <form>
              <v-text-field v-model="nuevoCurso.nombre" label="Nombre" required></v-text-field>
              <v-text-field v-model="nuevoCurso.img" label="URL de la imagen" required></v-text-field>
              <v-text-field v-model="nuevoCurso.cupos" label="Cupos del curso" required></v-text-field>
              <v-text-field v-model="nuevoCurso.inscritos" label="Inscritos en el curso" required></v-text-field>
              <v-text-field v-model="nuevoCurso.duracion" label="Duración del curso" required></v-text-field>
              <v-text-field v-model="nuevoCurso.fecha_registro" label="Fecha de registro" required></v-text-field>
              <v-text-field v-model="nuevoCurso.costo" label="Costo del curso" required></v-text-field>
              <v-textarea v-model="nuevoCurso.descripcion" label="Descripción del curso" required></v-textarea>

              <v-btn class="mr-4" @click="addNewCourse(); dialog = false">
                Agregar
              </v-btn>
            </form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
        <v-btn color="primary" @click="dialog = false;">Cerrar</v-btn>
      </v-dialog>
    </div>

    <div class="text-center">
      <v-dialog v-model="dialog2" width="auto">
        <CourseEdit></CourseEdit>
        <v-btn color="primary" @click="dialog2 = false;">Cerrar</v-btn>
      </v-dialog>
    </div>

    <v-chip class="ma-2" color="purple" text-color="white">
      Cantidad total de alumnos permitidos: {{ getMaxStudents }}
    </v-chip>

    <v-chip class="ma-2" color="blue" text-color="white">
      Cantidad total de alumnos inscritos: {{ getSignedStudents }}
    </v-chip>

    <v-chip class="ma-2" color="red" text-color="white">
      Cantidad total de cupos restantes: {{ getMaxStudents - getSignedStudents }}
    </v-chip>

    <v-chip class="ma-2" color="pink" text-color="white">
      Cantidad total de cursos terminados: {{ getFinishedCourses }}
    </v-chip>

    <v-chip class="ma-2" color="orange" text-color="white">
      Cantidad total de cursos activos: {{ getCurrentCourses }}
    </v-chip>

    <v-chip class="ma-2" color="yellow" text-color="black">
      Cantidad total de cursos: {{ getTotalCourses }}
    </v-chip>
  </div>
</template>

<script>
import { mapState, mapActions, mapGetters } from 'vuex'
import CourseEdit from '../components/CourseEdit.vue'
export default {
  name: 'AdminView',
  components: {
    CourseEdit
  },
  data() {
    return {
      dialog: false,
      dialog2: false,
      nuevoCurso: {
        id: 6,
        img: '',
        nombre: '',
        costo: '',
        duracion: '',
        cupos: '',
        inscritos: '',
        completado: '',
        fecha_registro: '',
        descripcion: ''
      }
    }
  },
  computed: {
    ...mapState(['cursos', 'id_course_edit']),
    ...mapGetters(['getMaxStudents', 'getSignedStudents', 'getFinishedCourses', 'getCurrentCourses', 'getTotalCourses'])
  },
  methods: {
    ...mapActions(['setIDCourseEdit']),
    showID(){
      console.log(this.id_course_edit)
    },
    addNewCourse() {
      this.nuevoCurso.completado = 'No'
      this.nuevoCurso.id++
      let data = {...this.nuevoCurso}
      this.$store.dispatch('addCourse', data);
      this.clean()
    },
    deleteCourse(id) {
      let response = confirm("¿Estás seguro(a) de eliminar este curso?")
      if (response) {
        this.$store.commit('DELETE_COURSE', id)
      }
    },
    clean(){
      this.nuevoCurso.img = ''
      this.nuevoCurso.nombre = ''
      this.nuevoCurso.costo = ''
      this.nuevoCurso.duracion = ''
      this.nuevoCurso.cupos = ''
      this.nuevoCurso.inscritos = ''
      this.nuevoCurso.completado = ''
      this.nuevoCurso.fecha_registro = ''
      this.nuevoCurso.descripcion = ''
    }
    
  }
}
</script>

<style scoped>
#title {
  font-size: 3em;
}

.addButton {
  margin-bottom: 2em;
}

th,
td {
  width: 300px;
}

.heroesTable {
  width: 1000px;
  margin-top: 50px;
  margin-left: 350px;
}
</style>
