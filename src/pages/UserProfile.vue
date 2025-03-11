<script setup>
import { useRoute } from 'vue-router';

const route = useRoute();

const title = "User Profile";
const username = route.params.username;
</script>

<template>
  <h1>{{ title }}</h1>
  <h2>{{ $route.params.username }}</h2>
  <h2>{{ username }}</h2>
</template>
