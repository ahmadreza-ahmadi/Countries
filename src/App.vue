<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <div class="min-vh-100 bg-light">
    <header
      class="d-flex p-3 justify-content-between align-items-center bg-white shadow-sm mb-4"
    >
      <nav class="container d-flex justify-content-between align-items-center">
        <h4>Where is the world?</h4>
        <button class="btn">Dark Mode</button>
      </nav>
    </header>
    <main class="container position-relative">
      <RouterView />
    </main>
  </div>
</template>
