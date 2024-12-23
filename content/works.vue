<template>
    <div>
      <!-- Loading state -->
      <div v-if="isLoading" class="flex items-center justify-center h-screen">
        <p>Loading...</p>
      </div>
  
      <!-- Error state -->
      <div v-else-if="hasError" class="flex items-center justify-center h-screen">
        <p>Error loading settings. Please try again later.</p>
      </div>
  
      <!-- Main content -->
      <div v-else class="h-screen relative">
        <!-- Background container -->
        <div class="relative h-full">
          <!-- Background image -->
          <div
            class="absolute inset-0 bg-cover bg-center"
            :style="{ backgroundImage: homepageSettings?.thumbnail ? `url(${homepageSettings.thumbnail})` : '' }"
          ></div>
  
          <!-- Content above the background -->
          <div class="relative z-10">
            <!-- Title Image -->
            <div class="gif-container animate-fade-up animate-ease-in">
              <img src="/img/title.png" alt="Title" />
            </div>
  
            <!-- Footer Image -->
            <div class="hmpg animate-fade-up animate-ease-in">
              <img src="/img/homepage-layer.png" alt="Background Layer" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  
  const router = useRouter();
  
  const homepageSettings = ref(null); // For homepage.json data
  const generalSettings = ref(null); // For settings.json data
  
  const isLoading = ref(true); // Loading state
  const hasError = ref(false); // Error state
  
  onMounted(async () => {
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
  
      // Handle homepage redirect
      if (homepageSettings.value.homepageredirect) {
        const redirectPath = homepageSettings.value.homepageredirect
          ? `/page/${homepageSettings.value.homepageredirect}`
          : null;
  
        if (redirectPath) {
          router.push(redirectPath);
        }
      } else {
        isLoading.value = false; // Stop loading when data is fetched
      }
    } catch (error) {
      hasError.value = true; // Show error message if fetching fails
      console.error('Error loading settings:', error);
    }
  });
  </script>
  
  <style>
  .gif-container {
    position: fixed;
    top: 10%;
    left: 50%;
    transform: translate(-50%, 0);
    display: flex;
    justify-content: center;
    align-items: center;
    mix-blend-mode: screen;
    filter: brightness(200%);
  }
  
  .gif-container img {
    max-width: 80%;
    height: auto;
  }
  
  .hmpg {
    position: fixed;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    pointer-events: none; /* Optional: Ensures clicks pass through to layers below */
    mix-blend-mode: multiply;
    filter: brightness(200%);
  }
  
  .hmpg img {
    width: 100%; /* Ensures the image takes the full width of the canvas */
    height: auto; /* Keeps aspect ratio */
    object-fit: cover; /* Ensures the image fills its container without distortion */
  }
  </style>