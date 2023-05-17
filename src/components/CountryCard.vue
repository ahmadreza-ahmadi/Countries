<script setup>
const props = defineProps(['name', 'population', 'region', 'capital', 'image'])
</script>

<template>
  <article>
    <img :src="image" :alt="name" class="bg-white" />
    <h2>{{ name }}</h2>
    <p>{{ population }}</p>
    <p>{{ region }}</p>
    <p>{{ capital }}</p>
  </article>
</template>

<style scoped>
img {
  width: 100%;
  /* height: 20px; */
}
</style>
