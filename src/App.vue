<script setup>
</script>

<template>
  <main>
    <div class="container">
      <form>
        <textarea name="post" id="post-textarea" rows="1" placeholder="Quoi de neuf ?"></textarea>
        <button type="submit">Poster</button>
      </form>
    </div>
  </main>
</template>

<style scoped>
.container {
  background-color: var(--color-bg-secondary);
  border-left: 1px solid var(--color-border);
  border-right: 1px solid var(--color-border);
  height: 100vh;
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
</style>
