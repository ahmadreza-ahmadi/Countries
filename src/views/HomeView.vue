<script setup>
import axios from 'axios'
import { ref, watch, computed, onMounted } from 'vue'
import CountryCard from '../components/CountryCard.vue'
import SearchBar from '../components/SearchBar.vue'
import SelectMenu from '../components/SelectMenu.vue'

// Datas
const countriesApi = ref()
const countries = ref()
const regions = ref([
  { id: 1, name: 'Asia' },
  { id: 2, name: 'Oceania' },
  { id: 3, name: 'Europe' },
  { id: 4, name: 'Americas' },
  { id: 5, name: 'Antarctic' },
  { id: 6, name: 'Africa' }
])

// Pagination variables
const currentPage = ref(1)
const countriesPerPage = 8
const pageLimit = computed(() => currentPage.value * countriesPerPage)
const pageOffset = computed(() => pageLimit.value - countriesPerPage)
const pagesNumber = ref()
let currentPageCountries = []

// Network and loading variables
const isLoading = ref(false)
const networkErrorMessage = ref('')

// Filters variables
const searchText = ref('')
const regionFilterValue = ref(0)
const selectedRegion = computed(() => {
  return regions.value.find((region) => region.id === regionFilterValue.value)
})

// Watchers
watch(currentPage, () => {
  setCurrentPageCountries()
})

watch(countries, () => {
  calculatePageNumber()
  setCurrentPageCountries()
})

watch(searchText, () => {
  regionFilterChangeHandler()
  searchChangeHandler()
  setCurrentPageCountries()
})

watch(regionFilterValue, () => {
  regionFilterChangeHandler()
  searchChangeHandler()
})

// Lifecycle methods
onMounted(async () => {
  await getCountries()
})

// Functions
async function getCountries() {
  isLoading.value = true
  try {
    const res = await axios.get('https://restcountries.com/v2/all')
    countriesApi.value = await res.data
    countries.value = countriesApi.value
    setCurrentPageCountries()
    calculatePageNumber()
  } catch (error) {
    networkErrorMessage.value = error.message
  }
  isLoading.value = false
}

function resetCountries() {
  countries.value = Object.values(countriesApi.value)
}

function setCurrentPageCountries() {
  currentPageCountries = countries.value.slice(
    pageOffset.value,
    pageLimit.value
  )
}

function searchChangeHandler() {
  currentPage.value = 1
  countries.value = countries.value.filter((country) =>
    country.name.toLowerCase().includes(searchText.value.toLowerCase())
  )
}

function regionFilterChangeHandler() {
  resetCountries()
  if (regionFilterValue.value /* has value... */) {
    // Filter countries based on selected region
    countries.value = countries.value.filter(
      (country) =>
        country.region.toLowerCase() === selectedRegion.value.name.toLowerCase()
    )
  }
}

function calculatePageNumber() {
  pagesNumber.value = Math.ceil(countries.value.length / countriesPerPage)
}
</script>

<template>
  <div>
    <!-- Loading -->
    <div
      v-if="isLoading"
      class="position-fixed top-0 start-0 w-100 h-100 bg-white d-flex justify-content-center align-items-center"
      style="z-index: 2"
    >
      <div class="spinner-border" role="status">
        <span class="sr-only"></span>
      </div>
    </div>

    <!-- Filters -->
    <div class="row justify-content-between">
      <!-- Search Filter -->
      <div class="col-6 col-md-4 col-lg-3">
        <SearchBar v-model="searchText" />
      </div>

      <!-- Region Filter -->
      <div class="w-auto">
        <SelectMenu v-model="regionFilterValue">
          <!-- Is that beeter to change value of a ref or not? -->
          <option
            v-if="!isLoading"
            v-for="region in regions"
            :key="region.id"
            :value="region.id"
          >
            {{ region.name }}
          </option>
        </SelectMenu>
      </div>
    </div>

    <!-- Cards Wrapper -->
    <div class="row justify-content-center g-5">
      <!-- Error Handlers -->
      <div class="text-center h5">
        <span v-if="countriesApi && !countries.length" class="text-primary">
          There is not country with name "{{ searchText }}" in
          {{ selectedRegion ? selectedRegion.name : 'all' }} region{{
            selectedRegion ? null : 's'
          }}
        </span>
        <span v-if="!countriesApi" class="text-danger">
          {{ networkErrorMessage }}
        </span>
      </div>

      <CountryCard
        v-for="country in currentPageCountries"
        :key="country.id"
        :country="country"
      />
    </div>

    <div
      v-if="countriesApi && countries.length > countriesPerPage"
      class="d-flex justify-content-center align-items-center p-4"
    >
      <button
        @click="currentPage = 1"
        class="btn border-end-0 btn-outline-primary rounded-0 rounded-start"
        :class="{ disabled: currentPage === 1 }"
      >
        First Page
      </button>
      <button
        @click="currentPage--"
        class="btn border-end-0 btn-outline-primary rounded-0"
        :class="{ disabled: currentPage === 1 }"
      >
        Prev
      </button>
      <button
        @click="currentPage++"
        class="btn border-end-0 btn-outline-primary rounded-0"
        :class="{ disabled: currentPage === pagesNumber }"
      >
        Next
      </button>
      <button
        @click="currentPage = pagesNumber"
        class="btn btn-outline-primary rounded-0 rounded-end"
        :class="{ disabled: currentPage === pagesNumber }"
      >
        Last Page
      </button>
    </div>
  </div>
</template>
