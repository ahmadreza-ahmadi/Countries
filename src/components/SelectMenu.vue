<script setup>
// --------------------
// ---> Propertise <---
// --------------------
const props = defineProps({
  modelValue: {
    type: Number,
    required: true
  }
})

// ----------------
// ---> Events <---
// ----------------
const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <select
    :value="modelValue"
    @change="$emit('update:modelValue', parseInt($event.target.value))"
    class="form-select w-auto py-2"
  >
    <option value="0" selected>All</option>
    <slot></slot>
  </select>
</template>
