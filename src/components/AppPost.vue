<script setup>
import { TrashIcon } from "@heroicons/vue/16/solid";
import { useRouter } from "vue-router"

const router = useRouter();

defineProps(["post"]);
const emit = defineEmits(["delete"]);

const goToUserProfile = () => {
  router.push({ name: "user-profile"});
}
</script>

<template>
  <article class="card">
    <img
      :src="post.user.avatar"
      :alt="`${post.user.username} avatar picture`"
      width="36"
      height="36"
      class="user-avatar"
    >
    <div class="post-content">
      <header class="post-header">
        <a @click="goToUserProfile" class="user-link">{{ post.user.username }}</a>
        <button class="delete-post" @click="emit('delete', post.id)">
          <TrashIcon />
        </button>
      </header>
      <p>{{ post.text }}</p>
    </div>
  </article>
</template>

<style scoped>
article {
  display: flex;
  gap: 0.75rem;
  padding: 0.75rem 1.5rem;
  overflow: hidden;
}
article p {
  white-space: pre-wrap;
}
article:hover .delete-post {
  display: block;
}

.post-content {
  overflow: hidden;
  width: 100%;
}
.post-header {
  display: flex;
  justify-content: space-between;
}

.user-avatar {
  border-radius: 50%;
  margin-top: 0.25rem;
  object-fit: cover;
}
.user-link {
  cursor: pointer;
  font-weight: bold;
}
.user-link:hover {
  text-decoration: underline;
}

.delete-post {
  background: none;
  border: none;
  cursor: pointer;
  display: none;
  width: 24px;
  color: var(--color-text-primary);
}
.delete-post:hover {
  color: var(--color-accent);
}
</style>
