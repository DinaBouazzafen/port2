<template>
  <div>
    <div class="pr-5">
      <Drawer />
    </div>
    <main class="mx-auto max-w-4xl space-y-6 px-4 mb-20">
      <h1 class="text-6xl font-bold road-rage-regular capitalize">Motion</h1>
      <div class="mb-10">
        <NuxtLink :to="`/page`">
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
              <path fill="currentColor" d="m6.921 12.5l5.793 5.792L12 19l-7-7l7-7l.714.708L6.92 11.5H19v1z"></path>
            </svg>
          </span>
          <span class="text-xs tinos-regular">My world</span>
        </NuxtLink>
      </div>
      <div class="masonry">
        <div 
          class="masonry-item" 
          v-for="post in filteredPosts" 
          :key="post.slug"
        >
          <div class="masonry-image">
            <NuxtImg
              :src="post.thumbnail"
              alt="Post thumbnail"
              class="w-full h-64 object-cover rounded-md mb-4"
              :width="750"
              :height="550"
            />
          </div>
          <NuxtLink :to="post._path" class="block">
            <div class="masonry-item-text p-4">
              <h2 class="text-3xl font-semibold mb-2 road-rage-regular">{{ post.title }}</h2>
              <p v-if="post.description" class="text-gray-300 line-clamp-3 text-xl grenze">{{ post.description }}</p>
            </div>
          </NuxtLink>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Fetch all posts and filter by the specific category (e.g., Graphic Design)
const { data: post } = await useAsyncData('post', () => {
  return queryContent('/page')
    .where({ category: 'motion' }) 
    .find()
})

// Assign the filtered posts to a ref
const filteredPosts = ref(post)
</script>
