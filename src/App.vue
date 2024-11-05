<template>
  <v-app>
    <!-- Mostrar el indicador de carga o error -->
    <div v-if="loading" class="loader">
      <v-progress-circular indeterminate color="primary"></v-progress-circular>
    </div>
    <div v-if="errorMessage" class="error-message">
      <v-alert type="error" dismissible @input="clearErrorMessage">
        {{ errorMessage }}
      </v-alert>
    </div>

    <v-main>
      <!-- Contenido principal -->
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      loading: false,        // Controla el estado de carga global
      errorMessage: ''        // Controla los mensajes de error globales
    };
  },
  methods: {
    startLoading() {
      this.loading = true;
    },
    stopLoading() {
      this.loading = false;
    },
    setErrorMessage(message) {
      this.errorMessage = message;
    },
    clearErrorMessage() {
      this.errorMessage = '';
    }
  },
  mounted() {
    // Escuchar el cambio de ruta y manejar la carga
    this.$router.beforeEach((to, from, next) => {
      this.startLoading(); // Mostrar el indicador de carga

      // Esperar medio segundo antes de permitir el cambio de página
      setTimeout(() => {
        next(); // Permitir el cambio de ruta
      }, 500);
    });

    this.$router.afterEach(() => {
      // Detener el indicador de carga 500ms después del cambio de ruta
      setTimeout(() => {
        this.stopLoading();
      }, 500);
    });
  }
};
</script>

<style>
.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.error-message {
  margin: 20px;
}
</style>
