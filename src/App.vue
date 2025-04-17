<script setup lang="ts">
import { Search } from "lucide-vue-next";

import type { Post, Author } from "@/types";

import Card from "./components/Card.vue";

import { ref, onMounted, computed } from "vue";

const posts = ref<Post[]>([]);
const authors = ref<Author[]>([]);
const searchQuery = ref<string>("");

async function getPosts() {
  const data = await fetch("https://jsonplaceholder.typicode.com/posts");

  return data.json();
}

async function getAuthors() {
  const data = await fetch("https://jsonplaceholder.typicode.com/users");

  return data.json();
}

onMounted(async () => {
  await Promise.all([getPosts(), getAuthors()]).then(
    ([postsArray, authorsArray]) => {
      posts.value = postsArray;
      authors.value = authorsArray;

      console.log(posts.value);
      console.log(authors.value);
    },
  );
});

const getAuthorName = (id: number) => {
  return authors.value.find((item) => item.id === id)?.name ?? "unknown author";
};

const filteredPost = computed(() => {
  if (!searchQuery.value.trim()) return posts.value;

  return posts.value.filter((post) => {
    const authorName = getAuthorName(post.userId).toLocaleLowerCase();

    return authorName.includes(searchQuery.value.toLocaleLowerCase());
  });
});
</script>

<template>
  <div class="search container">
    <div class="search__input-wrapper">
      <Search class="icon icon--search" :size="20" />
      <input
        class="search__input"
        type="text"
        minlength="3"
        maxlength="40"
        placeholder="Filter by author..."
        v-model="searchQuery"
      />
    </div>
    <div class="search__cards-grid">
      <Card
        v-if="posts.length > 0"
        v-for="post in filteredPost"
        :key="post.id"
        :card-title="post.title"
        :card-description="post.body"
        :card-author="getAuthorName(post.userId)"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
@use "@/styles/variables";

.search {
  min-height: 100vh;
  padding-bottom: 40px; 
  display: flex;
  row-gap: 20px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-block: 40px;
  height: 100%;

  &__input-wrapper {
    position: relative;
    width: 100%;
    max-width: 250px;
  }

  &__input {
    display: block;
    border: var(--border);
    border-radius: var(--rounded-base);
    padding-inline: 32px 20px;
    padding-block: 10px;
    width: 100%;
    min-height: var(--input-height);
    line-height: 1;
  }

  &__cards-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
  }
}

@media (width < 768px) {
  .search {
    &__cards-grid {
      grid-template-columns: repeat(1, 1fr);
    }
  }
}
</style>
