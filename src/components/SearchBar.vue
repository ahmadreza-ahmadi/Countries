<script setup>
import { computed } from 'vue';

const props = defineProps(['modelValue']);
const emit = defineEmits(['update:modelValue']);

const value = computed({
  get() {
    return props.modelValue;
  },
  set(value) {
    emit('update:modelValue', value);
  },
});
</script>

<template>
  <div>
    <input
      v-model="value"
      type="text"
      placeholder="Search for a country"
      class="form-control px-3 py-2"
    />
  </div>
</template>
