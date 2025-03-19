<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const username = ref("");
const email = ref("");
const password = ref("");

const isFormValid = computed(() => {
  return !!username.value.trim() && !!email.value.trim() && !!password.value.trim();
});

const isLoading = ref(false);

function register() {
  isLoading.value = true;

  fetch("https://posts-crud-api.vercel.app/register", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      username: username.value,
      email: email.value,
      password: password.value,
    }),
  })
    .then((res) => res.json())
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
  <form @submit.prevent="register">
    <h1 style="margin-bottom: 1rem">Créer un compte</h1>
    <input
      type="text"
      id="username"
      name="username"
      required
      v-model="username"
      placeholder="Nom d'utilisateur"
    />
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
      autocomplete="new-password"
      required
      v-model="password"
      placeholder="Mot de passe"
    />
    <button type="submit" :disabled="!isFormValid || isLoading" :class="{ loading: isLoading }">
      Créer un compte
    </button>
    <router-link to="/login" class="login_link">Je possède déjà un compte</router-link>
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
