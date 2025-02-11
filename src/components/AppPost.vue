<script setup>
defineProps(["post"]);
</script>

<template>
  <article>
    <img
      :src="post.user.avatar"
      :alt="`${post.user.username} avatar picture`"
      width="36"
      height="36"
      class="user-avatar"
    >
    <div class="post-content">
      <strong>{{ post.user.username }}</strong>
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
article:not(:last-child) {
  border-bottom: 1px solid var(--color-border);
}
article p {
  white-space: pre-wrap;
}
.post-content {
  overflow: hidden;
}

.user-avatar {
  border-radius: 50%;
  margin-top: 0.25rem;
  object-fit: cover;
}
</style>
