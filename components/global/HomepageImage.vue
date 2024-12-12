<template>
  <div>
    <!-- Loading and Error States -->
    <div v-if="isLoading" class="flex items-center justify-center h-screen">
      <p>Loading...</p>
    </div>
    <div v-else-if="hasError" class="flex items-center justify-center h-screen">
      <p>Error loading homepage settings. Please try again later.</p>
    </div>
  
    <!-- Main Content -->
    <div v-else :style="{ backgroundImage: settings.thumbnail ? `url(${settings.thumbnail})` : '' }" class="back bg-cover bg-center h-full">
      <div class="pr-5">
        <Drawer />
      </div>
      
      <!-- Info Section -->
      <div class="info flex flex-col items-center justify-center h-screen">
        <div class="container relative text-left p-1 lg:p-20">
          
          <!-- Background Video -->
          <video 
            autoplay 
            loop 
            muted 
            class="absolute inset-0 w-auto h-auto opacity-50 pointer-events-none"
            style="max-width: 100%; max-height: 50%; object-fit: cover;"
          >
          </video>
  
          <!-- Text and Navigation -->
          <div class="relative z-10 p-10 lg:p-20 bg-white dark:bg-black bg-opacity-80 dark:bg-opacity-80">
            <div class="text-xl font-bold">{{ settings?.homepage_title }}</div>
            <div class="text-sm opacity-80">{{ settings?.body }}</div>
  
            <div class="flex mt-10">
              <NuxtLink to="https://github.com/bureaupixel/" target="_blank">
                <div class="flex-col">
                  <span class="text-xs opacity-80">
                    This dataset is based on a Bureaupixel Github repository with a MIT License. Feel free to download and install it on your local machine.
                  </span>
                </div>
              </NuxtLink>
            </div>
          </div>
  
        </div>
      </div>
    </div>
  </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const settings = ref(null);  // Initialize as null for loading state
  const isLoading = ref(true);  // Loading state to show the content only after data is loaded
  const hasError = ref(false);  // Error state to show an error message if data loading fails
  
  onMounted(async () => {
    try {
      const response = await fetch('../_data/homepage.json');
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const jsonData = await response.json();
      settings.value = jsonData;
      isLoading.value = false;  // Stop loading when data is fetched
    } catch (error) {
      hasError.value = true;  // If there's an error, show the error message
      console.error('Error loading settings:', error);
    }
  });
  </script>
  