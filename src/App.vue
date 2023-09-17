<script setup>
import { ref, computed } from 'vue'; 
import Login from './components/login/Login.vue'; 
import Dashboard from './components/dashboard/Dashboard.vue';

//defining the routes to the other pages 
const routes = {
  '/': Login, 
  '/dashboard': Dashboard
}

const currentPath = ref(window.location.hash); 
window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash; 
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>

<template>
  <component :is="currentView" />
</template>

<style scoped>

</style>
