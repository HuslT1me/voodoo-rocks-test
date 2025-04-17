<script setup lang="ts">
import { ref } from "vue";

const { cardTitle, cardDescription, cardAuthor } = defineProps<{
  cardTitle: string;
  cardDescription: string;
  cardAuthor: string;
}>();

const count = ref(0);
</script>

<template>
  <div class="card">
    <a class="card__title" href="#">{{ cardTitle }}</a>
    <div class="card__description">
      <p>
        {{ cardDescription }}
      </p>
    </div>
    <span class="card__author">{{ cardAuthor }}</span>
  </div>
</template>

<style scoped lang="scss">
@use "@/styles/helpers/mixins" as *;

.card {
  display: grid;
  grid-template-rows: minmax(30px, 60px) auto auto;
  row-gap: 15px;
  border: var(--border);
  border-radius: var(--rounded-base);
  background-color: var(--color-white);
  padding: 20px;

  &__title {
    display: -webkit-box;
    overflow: hidden;
    color: var(--color-card-title);
    font-weight: 700;
    font-size: 20px;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    height: 100%;
    text-decoration: none;

    @include hover {
      color: var(--color-hover);
    }
  }

  &__author {
    margin-top: auto;
    color: var(--color-author);
    font-size: 14px;
  }
}
</style>
