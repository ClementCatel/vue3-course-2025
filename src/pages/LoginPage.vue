<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const email = ref("");
const password = ref("");

const isFormValid = computed(() => {
  return email.value.trim() && password.value.trim();
});

const isLoading = ref(false);

function login() {
  isLoading.value = true;

  fetch("https://posts-crud-api.vercel.app/login", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value,
    }),
  })
    .then((res) => {
      if (!res.ok) {
        throw new Error("Email ou mot de passe incorrect");
      }
      return res.json();
    })
    .then((data) => {
      localStorage.setItem("user", JSON.stringify(data));
      router.push("/");
    })
    .catch((err) => {
      console.error(err);
    })
    .finally(() => {
      isLoading.value = false;
    });
}
</script>

<template>
  <form @submit.prevent="login">
    <h1 style="margin-bottom: 1rem">Se connecter</h1>
    <input
      type="email"
      id="email"
      name="email"
      autocomplete="email"
      required
      v-model="email"
      placeholder="Email"
    />
    <input
      type="password"
      id="password"
      name="password"
      autocomplete="current-password"
      required
      v-model="password"
      placeholder="Mot de passe"
    />
    <button type="submit" :disabled="!isFormValid || isLoading" :class="{ loading: isLoading }">
      Connexion
    </button>
    <router-link to="/register" class="login_link">Je n'ai pas encore de compte</router-link>
  </form>
</template>

<style>
.login_link {
  display: block;
  text-align: center;
  margin-top: 1rem;
  font-weight: 500;
  font-size: 0.9rem;
}
</style>
