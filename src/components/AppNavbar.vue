<script setup>
import { ArrowRightStartOnRectangleIcon } from "@heroicons/vue/24/outline";
import { useUserData } from "@/composables/useUserData";

const { user } = useUserData();

function logout() {
  localStorage.removeItem("user");
  location.reload();
}
</script>

<template>
  <header class="navbar">
    <div class="navbar__user" v-if="user">
      <img :src="user.avatarUrl" alt="user avatar" class="avatar" height="36" width="36" />
      <b>{{ user.username }}</b>
    </div>

    <button class="btn-icon btn-icon--large" @click="logout">
      <ArrowRightStartOnRectangleIcon />
    </button>
  </header>
</template>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  border-bottom: 1px solid var(--color-border);
  position: sticky;
  top: 0;
  background-color: var(--color-bg-primary);
  z-index: 1;
}
.navbar__user {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
</style>
