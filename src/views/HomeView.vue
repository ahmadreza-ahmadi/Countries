<script setup>
import axios from 'axios';
import { ref, watch, onMounted } from 'vue';
import CountryCard from '../components/CountryCard.vue';
import SearchBar from '../components/SearchBar.vue';
import SelectMenu from '../components/SelectMenu.vue';

const countriesApi = ref();
const countries = ref();
const regions = ref([
  { id: 1, name: 'Asia' },
  { id: 2, name: 'Oceania' },
  { id: 3, name: 'Europe' },
  { id: 4, name: 'Americas' },
  { id: 5, name: 'Antarctic' },
  { id: 6, name: 'Africa' },
]);
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

onMounted(async () => {
  isLoading.value = true;
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
    // Find name of selected region in regions variable (reactive value) based on its id.
    const selectedRegion = regions.value.find(
      (region) => region.id === selectMenuText.value
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
    searchChangeHandler();
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
      <CountryCard
        v-for="country in countries"
        :key="country.id"
        :country="country"
      />
    </div>
  </div>
</template>
