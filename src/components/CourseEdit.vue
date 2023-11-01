<template>
    <v-card width="400px" class="mx-auto mt-5">
        <v-card-title primary-title>
            <h1 class="mx-auto">Editar curso</h1>
        </v-card-title>
        <v-card-text>
            <form>
                <v-text-field v-model="nuevoCurso.nombre" label="Nombre" required></v-text-field>
                <v-text-field v-model="nuevoCurso.img" label="URL de la imagen" required></v-text-field>
                <v-text-field v-model="nuevoCurso.cupos" label="Cupos del curso" required></v-text-field>
                <v-text-field v-model="nuevoCurso.inscritos" label="Inscritos en el curso" required></v-text-field>
                <v-text-field v-model="nuevoCurso.duracion" label="Duración del curso" required></v-text-field>
                <v-text-field v-model="nuevoCurso.fecha_registro" label="Fecha de registro" required></v-text-field>
                <v-text-field v-model="nuevoCurso.completado" label="Terminado" required></v-text-field>
                <v-text-field v-model="nuevoCurso.costo" label="Costo del curso" required></v-text-field>
                <v-textarea v-model="nuevoCurso.descripcion" label="Descripción del curso" required></v-textarea>

                <v-btn class="mr-4" @click="editCourse(); dialog = false">
                    Editar
                </v-btn>
            </form>
        </v-card-text>
        <v-card-actions>
            <v-spacer></v-spacer>
        </v-card-actions>
    </v-card>
    
</template>

<script>
import { mapState, mapActions, mapGetters } from 'vuex'
export default {
    name: 'CourseEdit',
    data() {
        return {
            nuevoCurso: {
                id: 6,
                img: '',
                nombre: '',
                costo: '',
                duracion: '',
                cupos: '',
                inscritos: '',
                completado: 'No',
                fecha_registro: '',
                descripcion: ''
            }
        }
    },
    computed: {
        ...mapState(['cursos', 'id_course_edit']),
        ...mapGetters(['getCourseByID'])
    },
    methods: {
        ...mapActions(['setIDCourseEdit', 'editSelectedCourse']),
        editCourse() {
            let data = { ...this.nuevoCurso, id: this.id_course_edit }
            this.editSelectedCourse(data)
            this.clean();
        },
        setCourseInForm() {
            let curso = this.getCourseByID(this.id_course_edit)
            this.nuevoCurso.nombre = curso.nombre
            this.nuevoCurso.img = curso.img
            this.nuevoCurso.cupos = curso.cupos
            this.nuevoCurso.inscritos = curso.inscritos
            this.nuevoCurso.duracion = curso.duracion
            this.nuevoCurso.fecha_registro = curso.fecha_registro
            this.nuevoCurso.costo = curso.costo
            this.nuevoCurso.descripcion = curso.descripcion
        },
        clean() {
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
    },
    watch:{
        id_course_edit(value){
            console.log(value)
            this.setCourseInForm()
        }
    },
    mounted() {
        this.setCourseInForm()
    }
}
</script>