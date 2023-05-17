<script setup>
const emits = defineEmits(['changeSearchInput'])
</script>

<template>
  <div>
    <input
      type="text"
      placeholder="Search for a country"
      @input="$emit('changeSearchInput', $event)"
    />
  </div>
</template>
