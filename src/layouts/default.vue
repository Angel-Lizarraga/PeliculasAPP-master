<template>
  <v-app>
    <!-- Barra de la aplicación -->
    <v-app-bar :elevation="2">
      <template v-slot:prepend>
        <!-- Icono de navegación para abrir/cerrar el cajón -->
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <!-- Título de la aplicación -->
      <v-app-bar-title>PelículasAPP</v-app-bar-title>
    </v-app-bar>

    <!-- Cajón de navegación -->
    <v-navigation-drawer v-model="drawer" app>
      <!-- Elemento de la lista principal del cajón -->
      <v-list-item title="PeliculasAPP" subtitle=""></v-list-item>
      <v-divider></v-divider> <!-- Divisor entre elementos -->
      <!-- Enlace a la página principal -->
      <v-list-item to="/" link>
        <v-list-item-icon>
          <v-icon>mdi-home</v-icon> <!-- Icono para Home -->
        </v-list-item-icon>
        <v-list-item-title>Home</v-list-item-title> <!-- Título para Home -->
      </v-list-item>
      <!-- Enlace a la página de películas -->
      <v-list-item to="/table" link>
        <v-list-item-icon>
          <v-icon>mdi-movie</v-icon> <!-- Icono para Películas -->
        </v-list-item-icon>
        <v-list-item-title>Películas</v-list-item-title> <!-- Título para Películas -->
      </v-list-item>
      <!-- Enlace a la página Acerca de -->
      <v-list-item to="/pagina2" link>
        <v-list-item-icon>
          <v-icon>mdi-information</v-icon> <!-- Icono para Acerca de -->
        </v-list-item-icon>
        <v-list-item-title>Acerca de</v-list-item-title> <!-- Título para Acerca de -->
      </v-list-item>
    </v-navigation-drawer>

    <!-- Contenido principal de la aplicación -->
    <v-main>
      <!-- Aquí se cargan las rutas definidas en el router -->
      <router-view />
    </v-main>

    <!-- Pie de página de la aplicación -->
    <AppFooter />
  </v-app>
</template>

<script setup>
// Importando el método ref de Vue para crear referencias reactivas
import { ref } from 'vue'

// Creando una referencia reactiva para el estado del cajón (abierto/cerrado)
const drawer = ref(false)
</script>

