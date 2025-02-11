<script setup>
import { computed, ref } from 'vue';

const text = ref("");
const trimmedText = computed(() => text.value.trim());

const posts = ref([]);

const submitPost = () => {
  posts.value.push(trimmedText.value);
  text.value = "";
};
</script>

<template>
  <main>
    <div class="container">
      <form @submit.prevent="submitPost">
        <textarea name="post" id="post-textarea" rows="1" placeholder="Quoi de neuf ?" v-model="text"></textarea>
        <button type="submit" :disabled="!trimmedText">Poster</button>
      </form>

      <hr>

      <section>
        <div v-if="!posts.length" class="feed__empty">
          <p>Aucun post pour le moment.</p>
        </div>

        <article v-for="post in posts" :key="post">
          <p>{{ post }}</p>
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
textarea {
  background: none;
  border: none;
  color: var(--color-text-primary);
  flex: 1;
  margin-bottom: 1rem;
  outline: none;
  padding: 0.5rem;
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
  opacity: 0.5;
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
  padding: 0.75rem 1.5rem;
  border-bottom: 1px solid var(--color-border);
}
</style>
