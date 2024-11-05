<template>
  <v-card class="mx-auto" max-width="600">
    <!-- Imagen que se mostrará en la tarjeta, su fuente se establece dinámicamente -->
    <v-img :src="imageUrl" height="400px"></v-img>
    <v-card-actions>
      <!-- Botón para cambiar la imagen, al hacer clic se llama a la función changeImage -->
      <v-btn color="primary" @click="changeImage">Siguiente</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      imageUrl: '' // Variable para almacenar la URL de la imagen
    };
  },
  mounted() {
    // Cuando el componente se monta, se llama a changeImage para establecer una imagen inicial
    this.changeImage();
  },
  methods: {
    changeImage() {
      // Genera un ID aleatorio entre 0 y 999 para la imagen
      const randomId = Math.floor(Math.random() * 1000);
      // Actualiza la URL de la imagen utilizando el ID aleatorio
      this.imageUrl = `https://picsum.photos/id/${randomId}/600/400`;
    }
  }
};
</script>

<style scoped>
.v-card {
  margin: 20px; /* Espaciado alrededor de la tarjeta */
}
</style>
