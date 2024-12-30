<template>
  <div class="flex animate-fade animate-once animate-delay-[500ms]">
    <div class="flex-auto w-1 pt-3"></div>
    <div class="flex-row pt-5 justify-end">
      <button style="margin-left: 16px" @click="drawer = true" class="bg-blue-900 dark:bg-slate-200 bg-opacity-20 hover:bg-opacity-90 dark:bg-opacity-10">
        <svg xmlns="http://www.w3.org/2000/svg" width="1.5em" height="1.5em" viewBox="0 0 24 24">
          <path
            fill="currentColor"
            d="M4 18h16q.425 0 .713-.288T21 17q0-.425-.288-.713T20 16H4q-.425 0-.713.288T3 17q0 .425.288.713T4 18Zm0-5h16q.425 0 .713-.288T21 12q0-.425-.288-.713T20 11H4q-.425 0-.713.288T3 12q0 .425.288.713T4 13Zm0-5h16q.425 0 .713-.288T21 7q0-.425-.288-.713T20 6H4q-.425 0-.713.288T3 7q0 .425.288.713T4 8Z"
            style="stroke-width: 4;" 
          ></path>
        </svg>
      </button>
    </div>
  </div>

  <el-drawer
    v-model="drawer"
    class="drawer min-w-fit max-w-screen"
    :direction="direction"
    :custom-class="'fixed-drawer'"
  >
    <!-- Drawer content goes here -->
    <div class="flex flex-col h-full">
      <div>
        <Titleblock />
      </div>
      <div class="flex-1 overflow-auto">
        <Navigations />
      </div>

      <!-- Footer is placed at the bottom of the drawer -->
      <div class="mt-auto footer">
        <Footer />
        <UtilNav />
      </div>
    </div>
  </el-drawer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import type { DrawerProps } from 'element-plus'
const drawer = ref(false)
const direction = ref<DrawerProps['direction']>('rtl')
</script>

<style>
.fixed-drawer {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000; /* Ensures it appears on top */
  background-color: rgba(255, 255, 255, 0.99);
}

.flex-row {
  display: flex;
  justify-content: flex-end;
}

.flex-row button {
  align-self: flex-start; /* Align the button at the top */
}

.mt-auto {
  margin-top: auto;
}
</style>
