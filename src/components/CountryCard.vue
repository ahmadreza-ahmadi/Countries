<script setup>
const props = defineProps(['name', 'population', 'region', 'capital', 'flag']);
</script>

<template>
  <div class="col-12 col-sm-6 col-md-4 col-lg-3 h-25">
    <div
      class="d-flex flex-column p-0 rounded shadow-sm overflow-hidden h-100 bg-white"
    >
      <img
        :src="props.flag"
        :alt="props.name"
        class="h-50 w-100 border-light border-bottom"
        style="object-fit: cover; object-position: top left"
      />
      <div class="d-flex flex-column p-4">
        <h4 class="text-truncate fw-bold">{{ props.name }}</h4>
        <small class="text-truncate fw-medium h6"
          >Population:
          <span class="fw-normal">{{ props.population }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Region: <span class="fw-normal">{{ props.region }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Capital: <span class="fw-normal">{{ props.capital }}</span></small
        >
      </div>
    </div>
  </div>
</template>
