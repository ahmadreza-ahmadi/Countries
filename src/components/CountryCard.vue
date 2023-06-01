<script setup>
// --------------------
// ---> Propertise <---
// --------------------
const props = defineProps({
  country: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="col-12 col-sm-6 col-md-4 col-lg-3" style="height: 350px">
    <div
      class="d-flex flex-column p-0 rounded shadow-sm overflow-hidden h-100 bg-white"
    >
      <img
        :src="country.flag"
        :alt="country.name"
        class="w-100 border-light h-50 border-bottom"
        style="object-fit: cover; object-position: center"
      />
      <div class="d-flex flex-column p-4">
        <h4 class="text-truncate fw-bold">{{ country.name }}</h4>
        <small class="text-truncate fw-medium h6"
          >Population:
          <span class="fw-normal">{{
            country.population.toLocaleString('en-US')
          }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Region: <span class="fw-normal">{{ country.region }}</span></small
        >
        <small class="text-truncate fw-medium h6"
          >Capital: <span class="fw-normal">{{ country.capital }}</span></small
        >
      </div>
    </div>
  </div>
</template>
