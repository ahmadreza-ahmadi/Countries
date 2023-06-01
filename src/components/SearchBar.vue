<script setup>
// --------------------
// ---> Propertise <---
// --------------------
const props = defineProps({
  modelValue: {
    type: String,
    required: true
  }
})

// ----------------
// ---> Events <---
// ----------------
const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <div>
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      type="text"
      placeholder="Search for a country"
      class="form-control px-3 py-2"
    />
  </div>
</template>
