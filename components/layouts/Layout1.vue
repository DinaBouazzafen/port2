<template>
  <div class="layout1">
    <cursor/>
    <ClientOnly>
      <div class="above relative max-h-screen overflow-hidden z-1">
        <!-- Video Background with Iframe -->
        <div class="video-container">
          <iframe
            v-if="data.video"
            :src="data.video"
            title="Video Background"
            frameborder="0"
            allow="autoplay; fullscreen; picture-in-picture"
            allowfullscreen
            class="video-frame"
            @load="handleVideoLoad"
          ></iframe>
        </div>

        <div class="container p-4">
          <div>
            <h1 class="text-black hnmi text-4xl md:text-6xl lg:text-8xl font-bold">{{ data.title }}</h1>
          </div>
        </div>

        <!-- Scroll Button -->
        <button class="scroll-button hnmi" @click="scrollToDrawer">More info</button>
      </div>

      <!-- Drawer Section -->
      <div class="drawer mx-auto p-4">
        <Drawer />
      </div>
      
      <!-- Main section -->
      <div class="container mx-auto p-4 animate-fade animate-once animate-delay-[500ms]" v-if="imageLoaded">
        <div class="">
          <NuxtLink :to="`/categories/${data.link}`" class="flex items-center">
            <span>
              <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                <path fill="currentColor" d="m6.921 12.5l5.793 5.792L12 19l-7-7l7-7l.714.708L6.92 11.5H19v1z"></path>
              </svg>
            </span>
            <span class="text-xs hn ml-2">Back</span>
          </NuxtLink>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-10 mt-6">
          <!-- First column -->
          <div>
            <h1 class="text-3xl md:text-3xl lg:text-6xl lg:mb-3 hnmi">{{ data.title }}</h1>
            <h1 v-if="data.subtitle" class="opacity-80 text-xl md:text-2xl lg:text-3xl hn pb-10">{{ data.subtitle }} INFO</h1>
            <p class="text-lg md:text-2xl lg:text-xl pb-5 font-bold">{{ data.description }}</p>
          </div>

          <!-- Second column -->
          <div>
            <ContentRenderer :value="data" />
          </div>
        </div>

        <!-- Image Gallery in the middle, spanning both columns -->
        <div v-if="data.imagegallery && data.imagegallery.showgallery" class="gallery mt-10 mx-auto">
          <ImageGallery class="grungy-gallery"/>
        </div>

        <!-- Second row -->
        <div v-if="data.related_page">
          <RelatedPages :relatedPages="data.related_page" />
        </div>
      </div>

      <!-- SEO metadata -->
      <Title>{{ data.title }}</Title>
      <Meta name="description" :content="data.description" />
      <Meta name="tags" :content="data.tags && Array.isArray(data.tags) ? data.tags.join(', ') : ''" />
      <Meta name="keywords" :content="data.tags && Array.isArray(data.tags) ? data.tags.join(', ') : ''" />
      <Meta property="og:title" :content="data.title" />
      <Meta property="og:description" :content="data.description" />
      <Meta property="og:video" :content="data.video" />
      <Meta property="og:url" :content="data.url" />
      <Meta property="og:type" content="article" />
    </ClientOnly>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Handle image loading state
const imageLoaded = ref(false);
const handleVideoLoad = () => {
  imageLoaded.value = true; // Set imageLoaded to true when video has loaded
};

// Scroll to the Drawer section
const scrollToDrawer = () => {
  const drawerElement = document.querySelector(".drawer");
  drawerElement.scrollIntoView({ behavior: "smooth" });
};

defineProps(['data', 'formatDate']);
</script>

<style scoped>

.video-container {

  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  scroll-snap-align: start;
}

.video-frame {
  position: absolute;
  top: 50%;
  left: 50%;
  min-width: 100vw; /* Stretch to cover the full width */
  min-height: 100vh; /* Stretch to cover the full height */
  width: auto;
  height: auto;
  transform: translate(-50%, -50%);
  object-fit: cover; /* Ensures the iframe fills the container */
  border: none;
  pointer-events: none; /* Prevents interaction with the iframe */
}

.gallery {
  text-align: center;
  max-width: 80%;
  padding: 20px;
  border-radius: 10px;
  position: relative;
  overflow: hidden;
}

.gallery-title {
  color: #fff;
  letter-spacing: 2px;
}

.grungy-gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  align-items: center;
}

.grungy-gallery img {
  width: 100%;
  max-width: 300px;
  height: auto;
  object-fit: cover;
  transition: transform 0.3s, filter 0.3s;
  border-radius: 4px;
}

.grungy-gallery img:hover {
  transform: scale(1.1);
}

.gallery::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 120%;
  background: url('/img/grungeBG.png') no-repeat center center;
  background-size: cover;
  opacity: 50%;
  z-index: 0;
  mix-blend-mode:difference;
}

@media (max-width: 768px) {
  .grungy-gallery img {
    max-width: 150px;
  }
}

.above{
  z-index: 11;
  mix-blend-mode: difference;
}
.drawer {
  position: relative;
  z-index: 10;
  margin-top: 0px; /* Slight overlap for design alignment */
  scroll-snap-align: start;
  margin-right: 0px;
}

.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #fff;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.imgs{
  mix-blend-mode: normal;

}

.scroll-button {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  padding: 10px 20px;
  background: url('/img/buttons/button-moreInfo.png') no-repeat center center;
  background-size: cover;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  color: transparent;
  width: 270px;
  height: 60px;
  z-index: 100;
  transition: transform 0.2s;
  filter: brightness(150%);
}

.scroll-button:hover {
  transform: translateX(-50%) scale(1.1);
}
</style>



