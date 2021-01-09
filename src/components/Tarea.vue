<template>
  <div>
    <h2 class="display-4 text-center">Listado de Tareas</h2>
    <hr />
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="card mt-4">
          <div class="card-body">
            <div class="input-group">
              <input
                type="text"
                class="form-control form-control-md"
                placeholder="Agregar Tarea"
                v-model="tarea"
              />
              <div class="input-group-append">
                <button
                  class="btn btn-success btn-md"
                  v-on:click="agregarTarea()"
                >
                  Agregar
                </button>
              </div>
            </div>
            <br />
            <div class="text-center">
              <div
                v-if="loading"
                class="spinner-border text-primary"
                role="status"
              >
                <span class="visually-hidden">Loading...</span>
              </div>
            </div>
            <h6 v-if="listTareas.length == 0">No hay tareas pendientes</h6>
            <ul v-if="!loading" class="list-group">
              <li
                v-for="(tarea, index) of listTareas"
                :key="index"
                class="list-group-item d-flex justify-content-between"
              >
                <span
                  class="cursor-pointer"
                  v-bind:class="{ 'text-success': tarea.estado }"
                >
                  <i
                    v-bind:class="[
                      tarea.estado ? 'fas fa-check-square' : 'far fa-square',
                    ]"
                    v-on:click="editarTarea(tarea, tarea.id)"
                  ></i>
                </span>
                <span
                  v-bind:class="[
                    tarea.estado ? 'text-decoration-line-through' : '',
                  ]"
                >
                  {{ tarea.nombre }}</span
                >
                <span class="text-danger cursor-pointer">
                  <i
                    class="fa fa-trash-alt"
                    v-on:click="eliminarTarea(tarea.id)"
                  ></i>
                </span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const url = "https://backend-tareas20210109010458.azurewebsites.net/api/Tarea/";
export default {
  name: "Tarea",
  data() {
    return {
      tarea: "",
      listTareas: [],
      loading: false,
    };
  },
  methods: {
    agregarTarea() {
      const tarea = {
        nombre: this.tarea,
        estado: false,
      };
      //this.listTareas.push(tarea);
      this.loading = true;
      axios
        .post(url, tarea)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
          this.loading = false;
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
      this.tarea = "";
    },
    eliminarTarea(id) {
      //this.listTareas.splice(index, 1);
      this.loading = true;
      axios
        .delete(url + id)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
          this.loading = false;
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
    },
    editarTarea(tarea, id) {
      //this.listTareas[index].estado = !tarea.estado;
      this.loading = true;
      axios
        .put(url + id, tarea)
        .then((response) => {
          console.log(response);
          this.obtenerTareas();
          this.loading = false;
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
    },
    obtenerTareas() {
      axios.get(url).then((response) => {
        console.log(response);
        this.listTareas = response.data;
      });
    },
  },
  created: function () {
    this.obtenerTareas();
  },
};
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
</style>
