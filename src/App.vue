<script setup lang="ts">
import TheHeader from "./components/TheHeader.vue";
import SearchBar from "./components/SearchBar.vue";
import UserProfile from "./components/UserProfile.vue";
import { ref } from "vue";

const user = ref(null);
const isUserNotFound = ref(false);
const nbSuccess = ref(0);

async function fetchUserData(username: string) {
  try {
    const userResponse = await (await fetch(`https://api.github.com/users/${username}`)).json()

    if (userResponse.message) {
      isUserNotFound.value = true;
    } else {
      nbSuccess.value++;
      if (user.value !== null) {
        await new Promise((res) => {
          user.value = null;
          setTimeout(() => {
            res(true)
          }, 250);
        })
      }
      user.value = userResponse;
      isUserNotFound.value = false;
    }
  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
  <TheHeader />

  <main>
    <SearchBar @submit-username="fetchUserData" :is-user-not-found="isUserNotFound" :nb-success="nbSuccess"/>
    <UserProfile :user="user"/>
  </main>
</template>

<style lang="scss">
@use '@/assets/scss/mixins';
@import './assets/base.scss';

@include mixins.tabletAndUp {
  body {
    display: flex;
    align-items: center;
  }
}

#app {
  width: min(730rem / 16, 100% - 3rem);
  margin-inline: auto;

  padding-block: 2rem;

  @include mixins.tabletAndUp {
    padding-block: 0;
    padding-bottom: 1.625rem;
  }
}

.card {
  background-color: var(--color-foreground);
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0px 16px 30px -10px rgba(70, 96, 187, 0.2);
}

html.dark .card {
  box-shadow: none;
}

main {
  display: flex;
  flex-direction: column;
  gap: 1rem;

  @include mixins.tabletAndUp {
    gap: 1.5rem;
  }
}
</style>
