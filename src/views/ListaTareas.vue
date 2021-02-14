<template>
    
  <v-card flat class="px-6">
    <v-card-text>
        <h1>LISTA DE TAREAS</h1>
        <lu v-for="(item, i) in tareas" :key="i">
          <li v-if=" item.id_usuario == user._id">
          ID: {{item.id_usuario}} - {{item.titulo}} - {{item.descripcion}}
          </li>
        </lu>

      <v-data-table
        :headers="headers"
        :items="tareas"
        
        :page.sync="page"
        :items-per-page="itemsPerPage"
        hide-default-footer
        class="elevation-1"
        @page-count="pageCount = $event"
      ></v-data-table>
      <div class="text-center pt-2">
        <v-pagination v-model="page" :length="pageCount"></v-pagination>
      </div>
      
    </v-card-text>
    
  </v-card>
  
</template>

<script>
import { mapState } from 'vuex'
import tareaService from "@/services/tarea.service";
export default {
  data() {
    return {
      page: 1,
      pageCount: 0,
      itemsPerPage: 10,
      headers: [
        { text: "ID", value: "id_usuario" },
        { text: "Título Tarea", value: "titulo" },
        { text: "Descripción tarea", value: "descripcion" },
      ],
      tareas: [],
    };
  },
  mounted() {
     this.getTareas();
  },
  methods: {
    getTareas() {
      console.log("Bandera 1");
      tareaService
        .all()
        .then((response) => {
          this.tareas = response.data;
          console.log("Bandera 2");
        })
        .catch((error) => {
          console.log(error.response.data.error);
          console.log("Bandera 3");
        });
    },
    getTareasById() {
      console.log("Bandera 4");
      tareaService
        .getById("601f038be38b845740bff7b6")
        .then((response) => {

          this.tareas = response.data;
          console.log("Bandera 5");
        })
        .catch((error) => {
          console.log(error.response.data.error);
          console.log("Bandera 6");
        });
    },
  },
  computed: {
    ...mapState(["token","user"]),
   
    
   },
};
</script>