<template>
  <div>
  <client-only>
    
    <div class="icon-container mb-5 ">
      <button
  class="icon"
  :class="{ active: colorMode.value === 'light' }"
  @click="setColorMode('light')"
>
  <span style="font-size: 1em; display: inline-block;">𖤓</span>
</button>
<button
  class="icon"
  :class="{ active: colorMode.value === 'dark' }"
  @click="setColorMode('dark')"
>
  <span style="font-size: 1em; display: inline-block;">☾</span>
</button>


      
    </div>
    
    <div class="w-full mb-5 hnb">
      <p class="description">Color mode: {{ colorMode.value }}</p>
    </div>




  </client-only>
  </div>
</template>

<script setup lang="ts">
// Initialize colorMode
const colorMode = useColorMode()

// Method to set the color mode preference
const setColorMode = (mode: string) => {
  colorMode.preference = mode
}
</script>

<style scoped>
.icon-container {
  display: flex;
  gap: 5px;
}
.icon {
  cursor: pointer;
  font-size: 2em;
  transition: color 0.3s;
}
.icon.active {
  color: var(--color-active); /* Define this variable in your global CSS */
}
</style>
