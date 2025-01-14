<template>
  <div>
    <div class="pr-5">
      <cursor />

    </div>
    <div class="pr-5 dr">
      <Drawer />

    </div>
    <main class="mx-auto space-y-6 px-4 mb-20">
      <!-- Title Image -->
      <div class="selected-works-title flex justify-center my-5">
        <img id="title" src="/img/selected-works.png" class="w-auto h-20 object-contain" />
      </div>
      <!-- Categories as Flipping Cards -->
      <div class="categories-container grid grid-cols-4 gap-8 mx-auto">
        <NuxtLink 
          v-for="category in categories" 
          :key="category.slug" 
          :to="`/categories/${category.slug}`"
          class="category-card relative block"
        >
          <div class="card-inner">
            <!-- Front: Thumbnail Image -->
            <div class="card-front">
              <img
                :src="category.thumbnail"
                alt="Category thumbnail"
                class="w-full h-full object-cover rounded-lg"
              />
            </div>
            <!-- Back: Category Title -->
            <div class="card-back">
              <h2 class="text-center text-white hnmi text-2xl font-bold">{{ category.name }}</h2>
              <p class="text-center text-gray hnmi text-l">{{ category.explanation }}</p>
            </div>
          </div>
        </NuxtLink>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const categories = ref([
  {
    name: 'Graphic Design',
    slug: 'graphicDesign',
    thumbnail: '/img/marvel.png',
    explanation: '',
  },
  {
    name: 'Mixed Media',
    slug: 'mixedMedia',
    thumbnail: '/img/poster.jpg',
    explanation: '',
  },
  {
    name: 'Motion Design',
    slug: 'motion',
    thumbnail: '/img/techno.png',
    explanation: '',
  },
  
])
</script>

<style scoped>
/* Title styling */
.selected-works-title img {
  width: 90%;
  height: auto;
  mix-blend-mode: difference;
  filter: brightness(300%);
  margin-left: -2%;
}


/* Categories container */
.categories-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  max-width: 1200px;
  margin: 0 auto;
  mix-blend-mode: difference;
}

/* Card styling */
.category-card {
  width: 300px;
  height: 400px;
  perspective: 1000px; /* Enables 3D perspective */
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d; /* Enables 3D transformations */
  transition: transform 0.6s ease-in-out; /* Smooth flipping effect */
}

.category-card:hover .card-inner {
  transform: rotateY(180deg); /* Flip the card */
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden; /* Hides the back when the front is visible */
  border-radius: 10px; /* Rounds the edges of the card */
  overflow: hidden;
}

/* Front: Thumbnail Image */
.card-front {
  transform: rotateY(0deg);
}

.card-front img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

/* Back: Category Name */
.card-back {
  transform: rotateY(180deg);
  background-color: rgb(129, 129, 129); /* Ensures the back face has a color */
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  font-size: 1.5rem;
  color: white;
  border-radius: 10px;
}

.dr{
  mix-blend-mode: difference;
}
</style>