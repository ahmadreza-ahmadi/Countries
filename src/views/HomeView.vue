<script setup>
import axios from 'axios';
import { onMounted } from 'vue';
import { ref, watch } from 'vue';
import CountryCard from '../components/CountryCard.vue';
import SearchFilter from '../components/SearchFilter.vue';
import RegionFilter from '../components/RegionFilter.vue';

const countriesAPI = ref();
const regionsAPI = ref();
const countries = ref();
const regions = ref(regionsAPI);
const isLoading = ref(false);

const searchText = ref('');
const regionFilterText = ref('');

// Get Countries
async function getCountries() {
  const res = await axios.get('https://restcountries.com/v3.1/all');
  countriesAPI.value = await res.data;

  countries.value = countriesAPI.value;
}

// Get Regions
async function getRegions() {
  const res = await axios.get(
    'https://API.thecompaniesAPI.com/v1/locations/continents'
  );
  regionsAPI.value = res.data.continents;
}

onMounted(async () => {
  isLoading.value = true;
  await getRegions();
  await getCountries();
  isLoading.value = false;
});

if (countries) {
  // Watch for searchText change
  watch(searchText, () => {
    countries.value = countriesAPI.value.filter((country) =>
      country.name.common.toLowerCase().includes(searchText.value.toLowerCase())
    );
  });

  // Watch for regionFilterText change
  watch(regionFilterText, () => {
    countries.value = countriesAPI.filter(
      (country) => country.region.toLowerCase() === regionFilterText.value
    );
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
        <SearchFilter
          :modelValue="searchText"
          @update:modelValue="(newValue) => (searchText = newValue)"
        />
      </div>

      <!-- Region Filter -->
      <div class="w-auto">
        <RegionFilter
          :regionFilterText="regionFilterText"
          @update:regionFilterText="(newValue) => (regionFilterText = newValue)"
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
        </RegionFilter>
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
