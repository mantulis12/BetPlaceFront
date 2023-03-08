<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="container">
    <div class="navbar">
      <nav class="nav-justified">
        <RouterLink class="btn btn-link" to="/">Home</RouterLink>
        <RouterLink class="btn btn-link" to="/about">Betting</RouterLink>
        <RouterLink class="btn btn-link" to="/betting-history">Betting History</RouterLink>
      </nav>
    </div>
  <RouterView />
  </div>
</template>

<style scoped>
header {

}
</style>
