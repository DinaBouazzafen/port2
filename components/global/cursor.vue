<template>
    <div>
      <!-- The div that follows the mouse -->
      <div
        class="follower"
        :style="{ top: `${position.y+30}px`, left: `${position.x+30}px` }"
      >
        <img src="/img/logo.png" alt="logo Image" />
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from "vue";
  
  // Reactive object to store mouse position
  const position = ref({ x: 0, y: 0 });
  
  const updatePosition = (event) => {
    // Use pageX and pageY to account for scrolling
    position.value.x = event.pageX ;
    position.value.y = event.pageY ;
  };
  
  onMounted(() => {
    // Add event listener when component is mounted
    window.addEventListener("mousemove", updatePosition);
  });
  
  onUnmounted(() => {
    // Remove event listener when component is unmounted
    window.removeEventListener("mousemove", updatePosition);
  });
  </script>
  
  <style scoped>
  .follower {
    position: absolute;
    width: 60px; 
    height: auto;
    pointer-events: none;
    transform: translate(-50%, -50%); 
    z-index: 900;
  }
  
  .follower img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
    
  </style>