<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { useRouter } from 'vue-router';

// try
let p5Instance = null;
const p5Container = ref(null);
const colorMode = useColorMode();


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
watch(
  () => colorMode.preference,
  (newMode) => {
    if (newMode === 'light') {
      console.log('Light mode is active');
      // Add your logic for light mode here
    } else if (newMode === 'dark') {
      console.log('Dark mode is active');
    }
  },
  { immediate: true } // Trigger immediately to handle the initial value
);

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
  <div>
    <cursor />
  </div>
  <div class="full-container">
    

    <!-- Loading state -->
    <div v-if="isLoading" class="flex items-center justify-center h-screen">
      <img src="/img/loader.gif" alt="Is it still loading?" class="w-10 h-10" />
    </div>

    <!-- Error state -->
    <div v-else-if="hasError" class="flex items-center justify-center h-screen">
      <p>Error loading settings. Please try again later.</p>
    </div>
    
    <!-- Main content -->
    <div v-else class="h-screen relative">
      <!-- You can now use colorMode.preference here -->
      <div class="absolute inset-0 background"></div>
      <div class="relative h-full power-of-light-bg">
        <!-- Background image -->
        <div class="absolute inset-0"></div>
        
        <!-- Drawer or other components -->
        <div class="relative z-10 pr-5">
          <Drawer />
        </div>

        <!-- Title image and p5 container overlay -->
        <div class="gif-container">
          <img id="titleGif" src="/img/title.png" />
        </div>
        <div class="transparent">
          <ColorMode />
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
  z-index: 0;        

}
.drawer{
  filter: brightness(200%);
color: aqua;
}
.background {
  position: fixed;  
  top: 0;  
  left: 0;  
  width: 100%;  
  height: 100%;  
  z-index: -1;        
  background-image: url('/img/homepage-background.png');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;

}

.power-of-light-bg {
  background: radial-gradient(
    circle 200px at var(--mouse-x) var(--mouse-y),
    rgb(255, 255, 255),
    rgba(255, 1, 1, 0.43)
  );
  z-index: 900;
  mix-blend-mode: difference;
}


.transparent{
  display: none;

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