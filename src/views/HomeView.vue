<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'
import CountryCard from '../components/CountryCard.vue'
import SearchFilter from '../components/SearchFilter.vue'
import RegionFilter from '../components/RegionFilter.vue'

const countries = ref()
const regions = ref()
const isLoading = ref(false)

async function fetchData() {
  isLoading.value = true

  // Get Countries
  await fetch('https://restcountries.com/v3.1/all')
    .then((res) => res.json())
    .then((data) => (countries.value = data))
    .catch((err) => console.log(err))

  // Get Regions
  await fetch('https://api.thecompaniesapi.com/v1/locations/continents')
    .then((res) => res.json())
    .then((data) => (regions.value = data.continents))
    .catch((err) => console.log(err))

  isLoading.value = false
}

onMounted(() => {
  fetchData()
})
</script>

<template>
  <div>
    <!-- Loading -->
    <div
      v-if="isLoading"
      class="position-fixed top-0 start-0 vw-100 h-100 bg-white d-flex justify-content-center align-items-center"
      style="z-index: 2"
    >
      <div class="spinner-border" role="status">
        <span class="sr-only"></span>
      </div>
    </div>

    <!-- Filters -->
    <div class="row justify-content-between mb-4">
      <!-- Search Filter -->
      <div class="col-3">
        <SearchFilter />
      </div>

      <!-- Region Filter -->
      <div class="w-auto">
        <RegionFilter>
          <option
            v-if="!isLoading"
            v-for="region in regions"
            :key="region.code"
            :value="region.code"
          >
            {{ region.name }}
          </option>
        </RegionFilter>
      </div>
    </div>

    <!-- Cards Wrapper -->
    <div class="row justify-content-center g-5">
      <CountryCard
        v-for="country in countries"
        :key="country.id"
        :name="country.name.common"
        :population="country.population"
        :region="country.region"
        :capital="country.capital"
        :flag="country.flags.svg"
      />
    </div>
  </div>
</template>
