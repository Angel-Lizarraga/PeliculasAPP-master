<template>
  <v-container>
    <!-- Mostramos el título de la película seleccionada si existe -->
    <div v-if="selectedMovie">
      <h3>Pelicula: {{ selectedMovie }}</h3>
    </div>

    <v-table>
      <thead>
        <tr>
          <th>Título</th>
          <th>Calificación</th>
          <th>IMDB</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <!-- Iterar sobre la lista de películas y crear una fila por cada una -->
        <tr v-for="(movie, index) in movies" :key="index">
          <!-- Mostrar el título de la película y permitir seleccionarlo -->
          <td @click="selectMovie(movie.movie)" style="cursor: pointer;">
            {{ movie.movie }}
          </td>
          <td>{{ movie.rating }}</td>
          <td>
            <!-- Enlace a la URL de IMDB que se abre en una nueva pestaña -->
            <a :href="movie.imdb_url" target="_blank">{{ movie.imdb_url }}</a>
          </td>
          <td>
            <!-- Icono de editar para abrir el diálogo de edición -->
            <v-icon small @click="openEditDialog(movie)">mdi-pencil</v-icon>
            <!-- Icono de borrar para abrir el diálogo de borrado -->
            <v-icon small @click="openDeleteDialog(movie)">mdi-delete</v-icon>
          </td>
        </tr>
      </tbody>

    </v-table>

    <!-- Diálogo de edición -->
    <v-dialog v-model="editDialog" max-width="500px">
      <v-card>
        <v-card-title>Editar Película</v-card-title>
        <v-card-text>
          <v-form ref="editForm">
            <!-- Campos para editar el título, la calificación y la URL de IMDB -->
            <v-text-field v-model="editMovie.movie" label="Título" />
            <v-text-field v-model="editMovie.rating" label="Calificación" />
            <v-text-field v-model="editMovie.imdb_url" label="IMDB URL" />
          </v-form>
        </v-card-text>

        <v-card-actions>
          <!-- Botón para guardar los cambios -->
          <v-btn color="primary" @click="submitEdit">Guardar</v-btn>
          <!-- Botón para cerrar el diálogo sin guardar -->
          <v-btn color="red" @click="editDialog = false">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Diálogo de borrado con estilo similar al de edición -->
    <v-dialog v-model="deleteDialog" max-width="500px">
      <v-card>
        <v-card-title>Borrar Película</v-card-title>
        <v-card-text>
          <p>¿Estás seguro de que deseas eliminar la película?</p>
          <v-form ref="deleteForm">
            <!-- Campos para mostrar los detalles de la película a eliminar (solo lectura) -->
            <v-text-field v-model="deleteMovie.movie" label="Título" readonly />
            <v-text-field v-model="deleteMovie.rating" label="Calificación" readonly />
            <v-text-field v-model="deleteMovie.imdb_url" label="IMDB URL" readonly />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <!-- Botón para confirmar la eliminación -->
          <v-btn color="red" @click="submitDelete">Eliminar</v-btn>
          <!-- Botón para cerrar el diálogo sin eliminar -->
          <v-btn color="primary" @click="deleteDialog = false">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-container>
</template>

<script>
export default {
  data() {
    return {
      movies: [], // Arreglo que almacenará las películas
      selectedMovie: '', // Título de la película seleccionada
      editDialog: false, // Estado del diálogo de edición
      deleteDialog: false, // Estado del diálogo de borrado
      editMovie: {}, // Objeto que almacenará los datos de la película en edición
      deleteMovie: {}, // Objeto que almacenará los datos de la película a eliminar
    };
  },
  mounted() {
    // Llamar a la función para obtener la lista de películas al montar el componente
    this.fetchMovies();
  },
  methods: {
    async fetchMovies() {
      try {
        // Realizar una solicitud a la API para obtener las películas
        const response = await fetch("https://dummyapi.online/api/movies");
        if (response.ok) {
          const data = await response.json();
          this.movies = data; // Almacenar los datos de películas en la propiedad 'movies'
        } else {
          console.error("Error al obtener los datos de la API:", response.statusText);
        }
      } catch (error) {
        console.error("Error al realizar la solicitud:", error);
      }
    },
    selectMovie(movieTitle) {
      // Establecer la película seleccionada
      this.selectedMovie = movieTitle;
    },
    // Abrir diálogo de edición
    openEditDialog(movie) {
      // Copiar los datos de la película seleccionada al objeto de edición
      this.editMovie = { ...movie };
      this.editDialog = true; // Abrir el diálogo de edición
    },
    // Abrir diálogo de borrado
    openDeleteDialog(movie) {
      // Copiar los datos de la película seleccionada al objeto de borrado
      this.deleteMovie = { ...movie };
      this.deleteDialog = true; // Abrir el diálogo de borrado
    },
    // Enviar edición
    submitEdit() {
      // Buscar la película por su ID en el arreglo de películas y actualizarla
      const index = this.movies.findIndex(m => m.id === this.editMovie.id);
      if (index !== -1) {
        this.movies[index] = { ...this.editMovie }; // Actualizar los datos de la película editada
      }
      this.editDialog = false; // Cerrar el diálogo de edición
    },
    // Confirmar borrado
    submitDelete() {
      // Filtrar la lista de películas para eliminar la película seleccionada
      this.movies = this.movies.filter(m => m.id !== this.deleteMovie.id);
      this.deleteDialog = false; // Cerrar el diálogo de borrado
    },
  },
};
</script>
