<script setup>
import { computed, ref } from 'vue';
import AppPost from '@/components/AppPost.vue';

const user = {
  username: "Clément Catel",
  avatar: "https://yakovmerkin.com/wp-content/uploads/2021/02/erwin-smith-2.jpg",
}

const text = ref("");
const trimmedText = computed(() => text.value.trim());

const posts = ref([]);
const orderedPosts = computed(() => {
  return posts.value.slice().sort((a, b) => b.createdAt - a.createdAt)
})

const submitPost = () => {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    text: trimmedText.value,
    createdAt: Date.now(),
    liked: false,
    user,
  };
  posts.value.push(newPost);
  text.value = "";
};

const deletePost = (id) => {
  posts.value = posts.value.filter((post) => post.id !== id);
}

const likePost = (id) => {
  const post = posts.value.find((post) => post.id === id);
  if (!post) return;
  post.liked = !post.liked;
}
</script>

<template>
  <main>
    <div class="container">
      <form @submit.prevent="submitPost" class="card">
        <textarea name="post" id="post-textarea" rows="1" placeholder="Quoi de neuf ?" maxlength="200" v-model="text"></textarea>
        <button type="submit" :disabled="!trimmedText">Poster</button>
      </form>

      <p v-if="!posts.length">Aucun post pour le moment.</p>

      <AppPost v-for="post in orderedPosts" :key="post.id" :post="post" @delete="deletePost" @like="likePost" />
    </div>
  </main>
</template>
