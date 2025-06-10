<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import MainScreen from './components/MainScreen.vue'
</script>

<template>
  <div class="w-full py-10 flex items-center justify-center flex-col">
    <h1 class="text-3xl text-center">Welcome to the Pokemon Memory Challenge</h1>
    <div class="w-full max-w-[1200px]">
      <MainScreen />
    </div>
  </div>
</template>
