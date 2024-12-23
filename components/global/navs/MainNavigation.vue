<!-- <template>
  <div>
    <div v-for="(posts, folderName) in folders" :key="folderName" class="">
      <ul>
        <li v-for="post in posts" :key="post.slug" class="pb-1 md:pb-2 lg:pb-3">
          <NuxtLink :to="post._path" class="fjalla text-xl lg:text-3xl xl:text-3xl">
            {{ post.title }}
          </NuxtLink>
        </li>
      </ul>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useAsyncData } from '#app';
import menus from '../../../public/_data/menus.json'; // Import menus.json

// Define a ref for storing grouped and sorted folders
const folders = ref({});

// Use the constant path in queryContent
const { data: foldersPosts } = await useAsyncData('folders', () =>
  queryContent('/')
    .sort({ numbernavigation: -1 }) // Sorting in descending order (highest first)
    .find()
);

onMounted(() => {
  // Filter out the 'tags' folder and posts where 'numbernavigation' is 0
  let filteredPosts = (foldersPosts.value || []).filter(post => post._dir !== 'tags' && post.numbernavigation !== 0);

  // Apply filtering based on menus.json and the 'collections' folder condition
  filteredPosts = filteredPosts.filter(post => {
    const folderName = post._dir;
    
    // Remove all files from 'collections' if 'excludefromnavigation' equals 2
    if (folderName === 'collections' && menus.submenu['excludefromnavigation'] === 2) {
      console.log(`Excluding folder: ${folderName} because 'excludefromnavigation' is set to 2`);
      return false; // Exclude 'collections' folder
    }

    // Other condition for excluding submenu
    if (menus.submenu['excludefromnavigation'] === 2) {
      return false; // Exclude based on the position value
    }

    return true; // Include other folders
  });

  // Group posts by folder name (_dir)
  const grouped = filteredPosts.reduce((acc, post) => {
    const folderName = post._dir; // Folder name is stored in _dir
    if (!acc[folderName]) {
      acc[folderName] = [];
    }
    acc[folderName].push(post);
    return acc;
  }, {});

  // Sort posts within each folder by 'numbernavigation' in descending order
  for (const folder in grouped) {
    grouped[folder].sort((a, b) => b.numbernavigation - a.numbernavigation);
  }

  // Assign the grouped and sorted posts to the folders ref
  folders.value = grouped;
});
</script> -->

<template>
  <div>
    <ul class="ml-5">
      <!-- homepage -->
      <li class="pb-6 md:pb-7 lg:pb-8 ">
        <NuxtLink to="/" class="hnmi text-2xl lg:text-4xl xl:text-4xl">
          Homepage
        </NuxtLink>
      </li>
      <!-- About Me -->
      <li class="pb-6 md:pb-7 lg:pb-8 ">
        <NuxtLink to="/aboutMe" class="hnmi text-2xl lg:text-4xl xl:text-4xl">
          About me
        </NuxtLink>
      </li>

      <!-- Selected Works (Link + Collapsible) -->
      <li class="pb-3 md:pb-5 lg:pb-6 flex items-center">
        <!-- Direct link to Selected Works page -->
        <NuxtLink to="/selected-works" class="hnmi text-2xl lg:text-4xl xl:text-4xl mr-2">
          Selected works
        </NuxtLink>
        <!-- Toggle button for sub-categories -->
        <button
          @click="toggleSelectedWorks"
          class="focus:outline-none text-xl lg:text-3xl xl:text-3xl"
        >
        ↯
        </button>
      </li>
      <li v-if="showSelectedWorks" class="ml-5 hn">
        <ul>
          <li class="pb-3">
            <NuxtLink to="/selected-works/category1">Graphic design</NuxtLink>
          </li>
          <li class="pb-3">
            <NuxtLink to="/selected-works/category2">Motion design</NuxtLink>
          </li>
          <li class="pb-3">
            <NuxtLink to="/selected-works/category3">Mixed media</NuxtLink>
          </li>
          <li class="pb-6">
            <NuxtLink to="/selected-works/category4">Random works</NuxtLink>
          </li>
        </ul>
      </li>

      <!-- Contact Me -->
      <li class="pb-6 md:pb-7 lg:pb-8 ">
        <NuxtLink to="/contact" class="hnmi text-2xl lg:text-4xl xl:text-4xl">
          Contact me
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// State to control the display of Selected Works submenu
const showSelectedWorks = ref(false);

function toggleSelectedWorks() {
  showSelectedWorks.value = !showSelectedWorks.value;
}
</script>

<style scoped>

</style>
