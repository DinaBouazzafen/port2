<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { useRouter } from 'vue-router';
import { useColorMode } from '@vueuse/core';

// Retrieve the global colorMode state
const colorMode = useColorMode();

let p5Instance = null;
const p5Container = ref(null);

// Reactive state
const router = useRouter();
const homepageSettings = ref(null);
const generalSettings = ref(null);
const isLoading = ref(true);
const hasError = ref(false);

// Mouse move handler
const handleMouseMove = (e: MouseEvent) => {
  const { innerWidth, innerHeight } = window;
  const xPercent = (e.clientX / innerWidth) * 100;
  const yPercent = (e.clientY / innerHeight) * 100;
  document.documentElement.style.setProperty('--mouse-x', `${xPercent}%`);
  document.documentElement.style.setProperty('--mouse-y', `${yPercent}%`);
};

onMounted(async () => {
  window.addEventListener('mousemove', handleMouseMove);

  try {
    // Fetch homepage.json
    const homepageResponse = await fetch('/_data/homepage.json');
    if (!homepageResponse.ok) {
      throw new Error(`Homepage settings error! status: ${homepageResponse.status}`);
    }
    homepageSettings.value = await homepageResponse.json();

    // Fetch settings.json
    const settingsResponse = await fetch('/_data/settings.json');
    if (!settingsResponse.ok) {
      throw new Error(`General settings error! status: ${settingsResponse.status}`);
    }
    generalSettings.value = await settingsResponse.json();

    isLoading.value = false;
  } catch (error) {
    hasError.value = true;
    console.error('Error loading settings:', error);
  }
});
</script>

<template>
  <div class="full-container">
    <cursor />

    <!-- Loading state -->
    <div v-if="isLoading" class="flex items-center justify-center h-screen">
      <p>Loading...</p>
    </div>

    <!-- Error state -->
    <div v-else-if="hasError" class="flex items-center justify-center h-screen">
      <p>Error loading settings. Please try again later.</p>
    </div>

    <!-- Main content -->
    <div v-else class="h-screen relative power-of-light-bg">
      <!-- You can now use colorMode.preference here -->
      <p class="fixed top-5 left-5 text-white">Current Color Mode: {{ colorMode.value }}</p>

      <div class="relative h-full">
        <!-- Background image -->
        <div class="absolute inset-0 bg-cover bg-center"></div>
        
        <!-- Drawer or other components -->
        <div class="relative z-10 pr-5">
          <Drawer />
        </div>

        <!-- Title image and p5 container overlay -->
        <div class="gif-container">
          <img id="titleGif" src="/img/title.png" />
        </div>

        <!-- Additional layer -->
        <div class="hmpg">
          <img src="/img/homepage-layer.png" />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
:root {
  --mouse-x: 50%;
  --mouse-y: 50%;
}

.full-container {
  position: relative;
  overflow: hidden;
  height: 100vh;
  width: 100%;
}

.power-of-light-bg {
  background: radial-gradient(
    circle 200px at var(--mouse-x) var(--mouse-y),
    rgb(255, 255, 255),
    rgba(255, 1, 1, 0.43)
  ),
  url('/img/homepage-background.png');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  background-blend-mode: difference;
  transition: background 0.2s;
}

.power-of-light-bg-dark {
  background: radial-gradient(
    circle 200px at var(--mouse-x) var(--mouse-y),
    rgb(255, 255, 255),
    rgba(255, 1, 1, 0.43)
  ),
  url('/img/homepage-background.png');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  background-blend-mode: difference;
  transition: background 0.2s;
}


.gif-container {
  position: fixed;
  display: flex;
  left: 18%;
  justify-content: center;
  align-items: center;
  mix-blend-mode: screen;
  filter: brightness(200%);
}

.gif-container img {
  width: 100%;
  height: auto;
}

.hmpg {
  position: fixed;
  bottom: 0;
  left: 0%;
  width: 100%;
  display: fixed;
  justify-content: center;
  align-items: center;
  pointer-events: none;
  mix-blend-mode: difference;
  filter: brightness(200%);
}

.hmpg img {
  width: 100%;
  height: auto;
  object-fit: cover;
}


</style>
