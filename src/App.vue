<script setup>
import AppNavbar from "@/components/AppNavbar.vue";
import { computed } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

// Afficher la barre de navigation uniquement sur les pages autres que /login et /register
const showNavbar = computed(() => {
  return !["/login", "/register"].includes(route.path);
});
</script>

<template>
  <AppNavbar v-if="showNavbar" />

  <main>
    <div class="container">
      <RouterView />
    </div>
  </main>
</template>
