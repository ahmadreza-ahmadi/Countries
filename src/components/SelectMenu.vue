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
  <select class="form-select w-auto py-2" v-model="value">
    <option value="" selected>All</option>
    <slot></slot>
  </select>
</template>
