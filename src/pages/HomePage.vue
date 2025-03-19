<script setup>
import { ref } from "vue";
import AppPost from "@/components/AppPost.vue";
import PostForm from "@/components/PostForm.vue";
import { useUserData } from "@/composables/useUserData";

const { user } = useUserData();

// Les likes de l'utilisateur sont gérés en local pour une meilleure réactivité cf. src/components/AppPost.vue
// La vraie donnée sera mise à jour après avoir refetch les posts
function handleLike(postId) {
  const post = posts.value.find((post) => post.id === postId);
  if (!post) return;
  const like = post.likes.find((like) => like.userId === user.id);
  // Si l'utilisateur a déjà liké le post, on retire son like sinon on l'ajoute
  if (like) {
    post.likes = post.likes.filter((like) => like.userId !== user.id);
  } else {
    post.likes.push({ userId: user.id });
  }
}

// Même chose que pour les likes, on retire le post en local avant de faire la requête cf. src/components/AppPost.vue
function handleDelete(postId) {
  posts.value = posts.value.filter((post) => post.id !== postId);
}

const posts = ref([]);

const loading = ref(false);
function fetchPosts() {
  loading.value = true;
  fetch("https://posts-crud-api.vercel.app/posts")
    .then((response) => response.json())
    .then((data) => {
      posts.value = data;
      loading.value = false;
    });
}

fetchPosts();
</script>

<template>
  <PostForm @success="posts.unshift($event)" />

  <p v-if="loading && !posts.length">Chargement...</p>
  <p v-else-if="!posts.length">Aucun post pour le moment.</p>

  <AppPost
    v-for="post in posts"
    :key="post.id"
    :post="post"
    @like="handleLike"
    @delete="handleDelete"
  />
</template>
