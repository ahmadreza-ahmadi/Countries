<script setup>
import { ref } from 'vue'
import countries from '../db/countries.json'
import CountryCard from '../components/CountryCard.vue'
import SearchCountry from '../components/SearchCountry.vue'
import FilterCountry from '../components/FilterCountry.vue'
const searchInput = ref('')
const filterInput = ref('')
const handleChnageSearchInput = (event) => {
  searchInput.value = event.target.value
}
const handleChnageFilterInput = (event) => {
  filterInput.value = event.target.value
}
</script>

<template>
  <main>
    <SearchCountry @changeSearchInput="handleChnageSearchInput" />
    <p>Search Input Value: {{ searchInput }}</p>
    <FilterCountry @changeFilterInput="handleChnageFilterInput" />
    <p>Filter Input Value: {{ filterInput }}</p>
    <div class="row gx-5">
      <CountryCard
        v-for="country in countries"
        :key="country.id"
        :name="country.name"
        :population="country.population"
        :region="country.region"
        :capital="country.capital"
        :image="country.image"
        class="col-sm-1 col-md-3 bg-white"
      />
    </div>
  </main>
</template>
