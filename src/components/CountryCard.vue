<script setup>
const props = defineProps(['country']);
</script>

<template>
  <div class="col-12 col-sm-6 col-md-4 col-lg-3">
    <div
      class="d-flex flex-column p-0 rounded shadow-sm overflow-hidden h-100 bg-white"
    >
      <img
        :src="props.country.flags.svg"
        :alt="props.country.name.common"
        class="h-50 w-100 border-light border-bottom"
        style="object-fit: cover; object-position: center"
      />
      <div class="d-flex flex-column p-4">
        <h4 class="text-truncate fw-bold">{{ props.country.name.common }}</h4>
        <small class="text-truncate fw-medium h6"
          >Population:
          <span class="fw-normal">{{
            props.country.population.toLocaleString('en-US')
          }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Region:
          <span class="fw-normal">{{ props.country.region }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Capital:
          <span class="fw-normal">{{ props.country.capital }}</span></small
        >
      </div>
    </div>
  </div>
</template>
