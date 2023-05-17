<script setup>
import { ref } from 'vue'
const regions = ref([
  {
    id: 1,
    name: 'asia',
  },
  {
    id: 2,
    name: 'africa',
  },
])
const emits = defineEmits(['changeFilterInput'])
</script>

<template>
  <div>
    <select @change="$emit('changeFilterInput', $event)">
      <option v-for="region in regions" :key="regions.id" :value="regions.name">
        {{ region.name }}
      </option>
    </select>
  </div>
</template>
