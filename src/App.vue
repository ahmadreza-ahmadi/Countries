<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <header class="navbar bg-light shadow-sm">
    <div
      class="container-fluid d-flex justify-content-between align-items-center"
    >
      <h4>Where is the world?</h4>
      <p>Dark Mode</p>
    </div>
  </header>
  <div class="container">
    <RouterView />
  </div>
</template>
