<script setup>
import { computed, ref } from 'vue';

const user = {
  username: "Clément Catel",
  avatar: "https://yakovmerkin.com/wp-content/uploads/2021/02/erwin-smith-2.jpg",
}

const text = ref("");
const trimmedText = computed(() => text.value.trim());

const posts = ref([]);

const submitPost = () => {
  const newPost = {
    id: posts.value.length + 1,
    text: trimmedText.value,
    user,
  };
  posts.value.unshift(newPost);
  text.value = "";
};
</script>

<template>
  <main>
    <div class="container">
      <form @submit.prevent="submitPost">
        <div class="form-group">
          <img
            :src="user.avatar"
            :alt="`${user.username} avatar picture`"
            width="36"
            height="36"
            class="user-avatar"
          >
          <textarea name="post" id="post-textarea" rows="1" placeholder="Quoi de neuf ?" v-model="text"></textarea>
        </div>
        <button type="submit" :disabled="!trimmedText">Poster</button>
      </form>

      <hr>

      <section>
        <div v-if="!posts.length" class="feed__empty">
          <p>Aucun post pour le moment.</p>
        </div>

        <article v-for="post in posts" :key="post.id">
          <div>
            <img
              :src="post.user.avatar"
              :alt="`${post.user.username} avatar picture`"
              width="36"
              height="36"
              class="user-avatar"
            >
          </div>
          <div>
            <strong>{{ post.user.username }}</strong>
            <p>{{ post.text }}</p>
          </div>
        </article>
      </section>
    </div>
  </main>
</template>

<style scoped>
.container {
  background-color: var(--color-bg-secondary);
  border-left: 1px solid var(--color-border);
  border-right: 1px solid var(--color-border);
  min-height: 100vh;
  margin: 0 auto;
  max-width: 640px;
}

form {
  display: flex;
  flex-direction: column;
  padding: 1rem 1.5rem;
  width: 100%;
}
.form-group {
  align-items: center;
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
  color: var(--color-text-secondary);
  cursor: pointer;
  font-size: 1rem;
  height: 40px;
  padding: 0 1rem;
}
button:enabled:hover {
  color: var(--color-text-primary);
}
button:disabled {
  cursor: not-allowed;
  opacity: 0.3;
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

article {
  border-bottom: 1px solid var(--color-border);
  display: flex;
  gap: 0.75rem;
  padding: 0.75rem 1.5rem;
}
article > p {
  white-space: pre-wrap;
}

.user-avatar {
  border-radius: 50%;
  object-fit: cover;
}
</style>
