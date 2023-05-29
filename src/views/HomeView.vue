<script setup>
import axios from 'axios';
import { ref, watch, computed, onMounted } from 'vue';
import CountryCard from '../components/CountryCard.vue';
import SearchBar from '../components/SearchBar.vue';
import SelectMenu from '../components/SelectMenu.vue';

const countriesApi = ref();
const regionsApi = ref();
const countries = ref();
const regions = ref(regionsApi);
const isLoading = ref(false);

// Filters Texts
const searchText = ref('');
const selectMenuText = ref('');

// Get Countries
async function getCountries() {
  const res = await axios.get('https://restcountries.com/v3.1/all');
  countriesApi.value = await res.data;

  countries.value = countriesApi.value;
}

// Get Regions
async function getRegions() {
  const res = await axios.get(
    'https://API.thecompaniesAPI.com/v1/locations/continents'
  );
  regionsApi.value = res.data.continents;
}

onMounted(async () => {
  isLoading.value = true;
  await getRegions();
  await getCountries();
  isLoading.value = false;
});

function resetCountries() {
  countries.value = countriesApi.value;
}

function searchChangeHandler() {
  countries.value = countries.value.filter((country) =>
    country.name.common.toLowerCase().includes(searchText.value.toLowerCase())
  );
}

function regionFilterChangeHandler() {
  resetCountries();
  if (selectMenuText.value /* has value... */) {
    searchText.value = '';
    // Find name of selected region in RegionAPI variable (reactive value) based on its code.
    const selectedRegion = regionsApi.value.find(
      (region) => region.code === selectMenuText.value
    );
    // Filter countries based on selected region
    countries.value = countries.value.filter(
      (country) =>
        country.region.toLowerCase() === selectedRegion.name.toLowerCase()
    );
  }
}

if (countries /* has some country... */) {
  // Watch for searchText variable (reactive value) changes
  watch(searchText, () => {
    regionFilterChangeHandler();
    searchChangeHandler();
  });

  // Watch for selectMenuText variable (reactive value) changes
  watch(selectMenuText, () => {
    regionFilterChangeHandler();
  });
}
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
        <SearchBar
          :modelValue="searchText"
          @update:modelValue="(newValue) => (searchText = newValue)"
        />
      </div>

      <!-- Region Filter -->
      <div class="w-auto">
        <SelectMenu
          :modelValue="selectMenuText"
          @update:modelValue="(newValue) => (selectMenuText = newValue)"
        >
          <!-- Is that beeter to change value of a ref or not? -->
          <option
            v-if="!isLoading"
            v-for="region in regions"
            :key="region.code"
            :value="region.code"
          >
            {{ region.name }}
          </option>
        </SelectMenu>
      </div>
    </div>

    <!-- Cards Wrapper -->
    <div class="row justify-content-center g-5">
      <CountryCard
        v-for="country in countries"
        :key="country.id"
        :country="country"
      />
    </div>
  </div>
</template>
