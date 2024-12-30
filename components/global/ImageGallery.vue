<template>
  <ClientOnly>
    <div class="container">
      <div v-if="post.folder !== 'page'">
        <FolderContent v-if="post.folder" :title="post.title" />
      </div>

      <!-- Carousel -->
      <div v-if="post.imagegallery.carouselgrid == 'carousel'">
        <p v-if="post.imagegallery.gallerytitle" class="text-xl font-bold pb-2">
          {{ post.imagegallery.gallerytitle }}
        </p>
        <p v-if="post.imagegallery.gallerytext" class="pb-2">{{ post.imagegallery.gallerytext }}</p>

        <el-carousel indicator-position="none" motion-blur>
          <el-carousel-item v-for="(item, index) in post.imagegallery.galleryImages" :key="index">
            <div class="relative h-full flex items-center justify-center">
              <!-- Show loader until the image is fully loaded -->
              <div v-if="imageLoading[index]" class="absolute inset-0 flex items-center justify-center bg-gray-200">
                <p>Loading image...</p>
              </div>
              <NuxtImg
                :src="item"
                alt="Gallery Image"
                loading="lazy"
                @load="imageLoaded(index)"
                @click="openFullscreen(item)" 
                class="w-full h-auto max-h-full object-contain cursor-pointer"
              />
            </div>
          </el-carousel-item>
        </el-carousel>
      </div>

      <!-- Grid -->
      <div v-if="post.imagegallery.carouselgrid == 'grid'">
        <p v-if="post.imagegallery.gallerytitle" class="text-xl font-bold pb-2">
          {{ post.imagegallery.gallerytitle }}
        </p>
        <p v-if="post.imagegallery.gallerytext" class="pb-2">{{ post.imagegallery.gallerytext }}</p>
      
        <!-- Grid Container -->
        <div
          class="masonry"
          :class="{ 'single-image': post.imagegallery.galleryImages.length === 1 }"
        >
          <div
            class="masonry-item"
            v-for="(item, index) in post.imagegallery.galleryImages"
            :key="index"
          >
            <div
              class="relative masonry-image"
              :class="{ 'overlay': post.imagegallery.galleryImages.length > 1 }"
            >
              <!-- Show loader until the image is fully loaded -->
              <div
                v-if="imageLoading[index]"
                class="absolute inset-0 flex items-center justify-center bg-gray-200"
              >
                <p>Loading image...</p>
              </div>
              <NuxtImg
                :src="item"
                alt="Gallery Image"
                loading="lazy"
                @click="openFullscreen(item)"
                @load="imageLoaded(index)"
                class="cursor-pointer w-full object-cover rounded-md"
              />
            </div>
          </div>
        </div>
      </div>

      <!-- Fullscreen Overlay -->
      <div v-if="isOverlayVisible" class="fullscreen-overlay" @click="closeFullscreen">
        <NuxtImg :src="currentImage" alt="Fullscreen Image" class="fullscreen-image" />
        <button @click="closeFullscreen" class="close-button">
          <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
            <path fill="currentColor" d="m6.4 18.308l-.708-.708l5.6-5.6l-5.6-5.6l.708-.708l5.6 5.6l5.6-5.6l.708.708l-5.6 5.6l5.6 5.6l-.708.708l-5.6-5.6z"></path>
          </svg>
        </button>
      </div>
    </div>
  </ClientOnly>
</template>


<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useAsyncData } from '#app';

// Define reactive properties for managing image loading state
const imageLoading = ref([]);

// Get the current route
const route = useRoute();
const actualPath = route.path.replace(/\/$/, '');

// Fetch post data
const { data: post } = await useAsyncData('post', () => queryContent(actualPath).findOne());

// State for managing the fullscreen overlay
const isOverlayVisible = ref(false);
const currentImage = ref('');

// Function to open the fullscreen overlay
const openFullscreen = (image) => {
  currentImage.value = image;
  isOverlayVisible.value = true;
};

// Function to close the fullscreen overlay
const closeFullscreen = () => {
  isOverlayVisible.value = false;
  currentImage.value = '';
};

// Function to handle image loading
const imageLoaded = (index) => {
  imageLoading.value[index] = false;
};

// Initialize the loading state for images
onMounted(() => {
  if (post.value && post.value.imagegallery && post.value.imagegallery.galleryImages) {
    imageLoading.value = post.value.imagegallery.galleryImages.map(() => true);
  }
});
</script>
<style scoped>
/* Target the left and right arrows */
.el-carousel {
  --el-carousel-arrow-background: rgb(255 0 0 / 50%);
  --el-carousel-arrow-hover-background: rgba(0, 251, 255, 0.5);
  --el-carousel-arrow-font-size: 18px;
}
.fullscreen-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(60, 60, 60, 0.182);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  cursor: zoom-out;
}

.fullscreen-image {
  width: 100%;
  height: 100%;
}

.close-button {
  position: absolute;
  top: 20px;
  right: 20px;
  background: none;
  border: none;
  cursor: pointer;
  color: #fff;
  font-size: 1.5rem;
}
.masonry {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

.masonry.single-image {
  display: flex;
  justify-content: center;
  align-items: center;
}

.masonry-item {
  width: 100%;
}

.masonry.single-image .masonry-item {
  max-width: 500px; /* Centered single image size */
}

/* Overlay Effect for Multiple Images */
.masonry-image.overlay::after {
  content: '';
  position: absolute;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.3); /* Overlay effect for multiple images */
  pointer-events: none;
  opacity: 1;
}

.masonry-image:not(.overlay)::after {
  display: none; /* No overlay for single images */
}

/* Ensure single images have full opacity immediately */
.masonry.single-image .masonry-image {
  opacity: 1;
}
</style>