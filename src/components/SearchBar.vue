<script setup lang="ts">
  import { onMounted, ref, watch } from 'vue';

  const username = ref('octocat');

  const props = defineProps<{
    isUserNotFound: boolean;
    nbSuccess: number
  }>()

  const emit = defineEmits<{
    (e: 'submitUsername', username: string): void;
  }>()

  watch(() => props.nbSuccess, () => {
    username.value = '';
  })

  onMounted(() => {
    emit('submitUsername', username.value);
  })
</script>

<template>
  <form class="card" @submit.prevent="emit('submitUsername', username)">
    <button type="submit">
      <span class="sr-only">Search</span>
      <img src="@/assets/images/icon-search.svg" class="search-icon" alt="">
    </button>
    <input v-model="username" type="text" placeholder="Search GitHub username..." autofocus>
    <span v-if="isUserNotFound" class="not-found">No results</span>
    <button type="submit" class="btn">Search</button>
  </form>
</template>

<style scoped lang="scss">
  @use '@/assets/scss/mixins';

  form {
    padding: 7px;
    display: flex;
    align-items: center;
    gap: 0.5rem;

    @include mixins.tabletAndUp {
      padding: 10px;
      gap: 1.5rem;
    }

    .search-icon {
      width: 1.25rem;
      aspect-ratio: 1;
      margin-left: 9px;

      @include mixins.tabletAndUp {
        width: 1.5rem;
        margin-left: 22px;
      }
    }

    input {
      background-color: transparent;
      border: none;
      flex-grow: 1;
      flex-basis: 0;
      line-height: 25px;
      color: var(--color-text-input);
      outline: none;
      caret-color: var(--color-accent);

      &::placeholder {
        color: var(--color-text);
      }

      @include mixins.tabletAndUp {
        font-size: 1.125rem;
      }
    }

    .not-found {
      color: hsl(0 91% 62%);
      font-weight: 700;
      flex-shrink: 0;

      @include mixins.tabletAndUp {
        font-size: calc(15rem / 16);
      }
    }

    .btn {
      background-color: var(--color-accent);
      color: var(--color-white);
      border-radius: 10px;
      font-weight: 700;
      font-size: calc(14rem / 16);
      line-height: 1.5;
      padding: 0.75rem 1rem;
      transition: background-color 0.3s ease-out;

      &:where(:hover, :focus-visible) {
        background-color: var(--color-accent-hover);
      }

      @include mixins.tabletAndUp {
        font-size: 1rem;
        padding: calc(13rem / 16) 1.5rem;
      }
    }
  }
</style>