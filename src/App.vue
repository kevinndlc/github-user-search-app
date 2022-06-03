<script setup lang="ts">
import TheHeader from "./components/TheHeader.vue";

</script>

<template>
  <TheHeader />

  <main>
    <TheWelcome />
  </main>
</template>

<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin-inline: auto;
  padding: 2rem;
}

a,
.accent {
  text-decoration: none;
  color: var(--color-accent);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}
</style>
