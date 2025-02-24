<script setup>
import { computed, ref } from 'vue';
import AppPost from './components/AppPost.vue';

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
    user,
  };
  posts.value.push(newPost);
  text.value = "";
};

const deletePost = (id) => {
  posts.value = posts.value.filter((post) => post.id !== id);
}
</script>

<template>
  <main>
    <div class="container">
      <form @submit.prevent="submitPost" class="card">
        <div class="form-group">
          <img
            :src="user.avatar"
            :alt="`${user.username} avatar picture`"
            width="36"
            height="36"
            class="user-avatar"
          >
          <textarea name="post" id="post-textarea" rows="1" placeholder="Quoi de neuf ?" maxlength="200" v-model="text"></textarea>
        </div>
        <button type="submit" :disabled="!trimmedText">Poster</button>
      </form>

      <p v-if="!posts.length" class="feed__empty">Aucun post pour le moment.</p>

      <AppPost v-for="post in orderedPosts" :key="post.id" :post="post" @delete="deletePost" />
    </div>
  </main>
</template>

<style scoped>
.container {
  min-height: 100vh;
  margin: 0 auto;
  max-width: 640px;
  padding: 0 1rem;
}

.card {
  background-color: var(--color-bg-secondary);
  border-radius: 10px;
  border: 1px solid var(--color-border);
  margin-bottom: 1rem;
}

form {
  display: flex;
  flex-direction: column;
  margin-top: 1rem;
  padding: 1rem 1.5rem;
  width: 100%;
}
.form-group {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 1rem;
}
textarea {
  background: none;
  border: none;
  color: var(--color-text-primary);
  flex: 1;
  outline: none;
  padding: 0.5rem 0;
  resize: none;
  field-sizing: content;
}
button {
  align-self: flex-end;
  background: none;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  color: var(--color-text-primary);
  cursor: pointer;
  font-size: 1rem;
  height: 40px;
  padding: 0 1rem;
}
button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}

hr {
  border: none;
  border-top: 1px solid var(--color-border);
  margin: 0;
}

.feed__empty {
  padding: 1.5rem;
  text-align: center;
}

.user-avatar {
  border-radius: 50%;
  object-fit: cover;
}
</style>
