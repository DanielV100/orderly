<script setup>
import { ref, computed } from 'vue'; 
import Login from './components/login/Login.vue'; 
import Dashboard from './components/dashboard/Dashboard.vue';
import Header from './components/header/Header.vue';
import NewGoalkeeper from './components/goalkeeper/new_goalkeeper/NewGoalkeeper.vue';
import NewExercise from './components/exercises/new_exercise/NewExercise.vue';
const isShowingHeader = ref(false); 

//defining the routes to the other pages 
const routes = {
  '/': Login, 
  '/dashboard': Dashboard, 
  '/addgoalie': NewGoalkeeper, 
  '/addexercise': NewExercise

}

const currentPath = ref(window.location.hash); 
window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash; 
})

const currentView = computed(() => {
  let current = routes[currentPath.value.slice(1) || '/']; 
  //header shouldn't be shown in login page
  if(current.__name !== 'Login') {
    isShowingHeader.value = true; 
  } else {
    isShowingHeader.value = false; 
  }
  return  current || NotFound; 
})

</script>

<template>
  <Header v-if="isShowingHeader"/>
  <component :is="currentView" />
</template>

<style scoped>

</style>
