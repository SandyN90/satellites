
<template>
  <div class="flex justify-center max-w-lg sm:max-w-xl md:max-w-3xl lg:max-w-5xl mt-5 sm:mt-10 lg:mt-20 mx-auto">
    <SearchBar @search-query-result="getResult($event)" />
    <Filter @select-filter="newFilter = $event" />
  </div>
  <SearchResults v-show="searchedResult.length" :search-results="searchedResult" />
</template>

<script setup lang="ts">
import SearchBar from './SearchBar.vue';
import Filter from './Filter.vue';
import SearchResults from './SearchResults.vue';
import satelliteJSON from "../assets/satellites.json"
import { onMounted, ref, computed } from 'vue';

interface SatelliteData {
  name: string | null;
  countryCode: string | null;
  decayDate: string | null;
  intlDes: string | null;
  launchDate: string | null;
  noradCatId: string;
  objectType: string | null;
  orbitCode: string | null
}

const jsonData: SatelliteData[] = Object.values(satelliteJSON);
const searchedResult = ref<SatelliteData[]>([]);
const newFilter = ref("");

const filteredData = computed(() => newFilter.value === "" ? jsonData : jsonData.filter((satellite) => satellite[newFilter.value as keyof SatelliteData]));

const getResult = (query: string) => {
  searchedResult.value = query === ''
    ? filteredData.value
    : filteredData.value.filter((satellite) => {
      return (satellite.name?.toLowerCase().match(query.toLowerCase()) || satellite.noradCatId.match(query))
    })
};

onMounted(() => {
  console.log(jsonData);
})
</script>
