<template>
  
  <cursor />
  <div>
      <div class="dr pr-5">
        <Drawer />
      </div>
      <main class="mx-auto max-w-4xl space-y-6 px-4 mb-20">
        <h1 class="text-6xl font-bold hnmi capitalize">Graphic Design</h1>
        <div class="mb-10">
            <NuxtLink :to="`/page`" class="flex items-center">
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                  <path fill="currentColor" d="m6.921 12.5l5.793 5.792L12 19l-7-7l7-7l.714.708L6.92 11.5H19v1z"></path>
                </svg>
              </span>
              <span class="text-xs hn ml-2">Works</span>
            </NuxtLink>
        </div>
        <div class="grid grid-cols-3 gap-6">
          <NuxtLink 
            v-for="post in filteredPosts" 
            :key="post.slug" 
            :to="post._path"
            class="card relative block"
          >
            <div class="card-inner">
              <!-- Front: Thumbnail Image -->
              <div class="card-front">
                <NuxtImg
                  :src="post.thumbnail"
                  alt="Post thumbnail"
                  class="w-full h-full object-cover rounded-lg"
                  :width="750"
                  :height="550"
                />
              </div>
              <!-- Back: Title and Description -->
              <div class="card-back p-4 flex flex-col items-center justify-center">
                <h2 class="text-center text-white hnmi text-xl font-bold mb-2">{{ post.title }}</h2>
                <p v-if="post.description" class="text-center text-gray-300 text-sm">{{ post.description }}</p>
              </div>
            </div>
          </NuxtLink>
        </div>
      </main>
    </div>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  
  const { data: posts, error } = await useAsyncData('post', () => {
    return queryContent('/page').where({ category: 'graphic-design' }).find()
  })
  
  const filteredPosts = computed(() => posts.value || [])
  
  if (error.value) {
    console.error('Error fetching posts:', error.value)
  }
  </script>

<style scoped>
/* Container styling */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  max-width: 1200px;
  margin: 0 auto;
}

/* Card styling */
.card {
  width: 100%;
  height: 350px;
  perspective: 1000px;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.6s ease-in-out;
}

.card:hover .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  overflow: hidden;
}

/* Front: Thumbnail Image */
.card-front {
  transform: rotateY(0deg);
}

/* Back: Title and Description */
.card-back {
  transform: rotateY(180deg);
  background-color: rgb(45, 45, 45);
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 1rem;
  border-radius: 10px;
}

.dr{
  mix-blend-mode: difference;
}
</style>