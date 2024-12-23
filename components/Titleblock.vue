  <template>
    
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const settings = ref({});
  // Use useFetch to load the JSON data from the public folder
  const { data, error } = await useFetch('/_data/settings.json');
  
  // Set the data to the settings ref
  if (data.value) {
    settings.value = data.value;
  } else if (error.value) {
    console.error('Error loading settings:', error.value);
  }
  </script>