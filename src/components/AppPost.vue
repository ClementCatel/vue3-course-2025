<script setup>
import { TrashIcon, PencilSquareIcon, HeartIcon } from "@heroicons/vue/24/outline";
import { HeartIcon as SolidHeartIcon } from "@heroicons/vue/24/solid";
import { useRouter } from "vue-router"

const router = useRouter();

const props = defineProps(["post"]);
const emit = defineEmits(["delete", "like"]);

const goToUserProfile = () => {
  router.push({ name: "user-profile", params: { username: props.post.user.username } });
}
</script>

<template>
  <article class="card">
    <header>
      <img
        :src="post.author.avatarUrl"
        :alt="`${post.author.username} avatar picture`"
        width="36"
        height="36"
        class="avatar"
      >
      <a @click="goToUserProfile">{{ post.author.username }}</a>
      <small>{{ new Date(post.createdAt).toLocaleDateString() }}</small>
    </header>

    <p>{{ post.content }}</p>

    <footer>
      <button class="btn-icon" :class="{'active': post.liked}" @click="() => emit('like', post.id)">
        <component :is="post.liked ? SolidHeartIcon : HeartIcon" />
        <!-- <SolidHeartIcon v-if="post.liked" />
        <HeartIcon v-else /> -->
      </button>
      <div class="spacer"></div>
      <button class="btn-icon">
        <PencilSquareIcon />
      </button>
      <button @click="() => emit('delete', post.id)" class="btn-icon">
        <TrashIcon />
      </button>
    </footer>
  </article>
</template>
