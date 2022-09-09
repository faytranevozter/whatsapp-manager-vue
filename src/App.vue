<script setup>
import { RouterLink, RouterView } from 'vue-router'
import NavbarView from './views/partial/NavbarView.vue';
</script>

<template>
  <NavbarView />
  <div class="container mx-auto px-4">
    <RouterView />
  </div>
</template>
