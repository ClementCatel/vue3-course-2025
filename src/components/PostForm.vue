<script setup>
import { ref, computed } from "vue";
import { useUserData } from "@/composables/useUserData";

const emit = defineEmits(["success"]);

const text = ref("");
const trimmedText = computed(() => text.value.trim());

function handleSubmit() {
  // const userData = JSON.parse(localStorage.getItem("user"));
  // const token = userData.token;
  // Ici, on utilise le composable useUserData pour récupérer le token. cf. src/composable/useUserData.js
  const { token, user } = useUserData();

  fetch("https://posts-crud-api.vercel.app/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${token}`,
      // Le token JWT est envoyé dans l'en-tête Authorization
      // 'Bearer authentication' est Schéma d'authentification HTTP
      // Le nom "Bearer authentication" (authentification du porteur) peut être compris comme « donner accès au porteur de ce jeton ».
    },
    body: JSON.stringify({
      content: trimmedText.value,
    }),
  })
    .then((res) => res.json())
    .then((data) => {
      emit("success", {
        id: data.id,
        content: data.content,
        createdAt: data.createdAt,
        likes: [],
        authorId: data.authorId,
        author: {
          id: user.id,
          username: user.username,
          avatarUrl: user.avatarUrl,
        },
      });
      text.value = "";
    })
    .catch((err) => {
      console.error(err);
    });
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="card">
    <textarea
      name="post"
      id="post-textarea"
      rows="1"
      placeholder="Quoi de neuf ?"
      maxlength="200"
      v-model="text"
    ></textarea>
    <button type="submit" :disabled="!trimmedText" style="align-self: flex-end">Poster</button>
  </form>
</template>
