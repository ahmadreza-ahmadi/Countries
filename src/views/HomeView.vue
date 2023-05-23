<script setup>
import axios from 'axios'
import { onMounted } from 'vue'
import { ref, watch } from 'vue'
import CountryCard from '../components/CountryCard.vue'
import SearchFilter from '../components/SearchFilter.vue'
import RegionFilter from '../components/RegionFilter.vue'

const countriesAPI = ref()
const regionsAPI = ref()
const countries = ref(countriesAPI)
const regions = ref(regionsAPI)
const isLoading = ref(false)

const searchText = ref()

// Get Countries
async function getCountries() {
  isLoading.value = true

  const res = axios.get('https://restcountries.com/v3.1/all')
  countriesAPI.value = res.data

  isLoading.value = false
}

async function getRegions() {
  isLoading.value = true

  const res = await axios.get(
    'https://API.thecompaniesAPI.com/v1/locations/continents'
  )
  regionsAPI.value = res.data.continents

  isLoading.value = false
}

onMounted(() => {
  getCountries()
  getRegions()
})

watch(searchText, () => {
  countries.value = countriesAPI.filter((country) =>
    country.name.toLowerCase().includes(searchText.value.toLowerCase())
  )
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
      <em v-if="!countries" class="text-danger h5 text-center">
        Oops!... Something went wrong in loading countries
      </em>
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
