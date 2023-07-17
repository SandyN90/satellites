
<template>
  <div class="flex justify-center max-w-lg sm:max-w-xl md:max-w-3xl lg:max-w-5xl mt-5 sm:mt-10 lg:mt-20 mx-auto">
    <SearchBar @search-query-result="getResult($event)" />
  </div>
  <SearchResults v-show="searchedResult.length" :search-results="searchedResult" />
</template>

<script setup lang="ts">
import SearchBar from './SearchBar.vue';
import SearchResults from './SearchResults.vue';
import satelliteJSON from "../assets/satellites.json"
import { onMounted, ref } from 'vue';

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

const jsonData = Object.values(satelliteJSON);
const searchedResult = ref<SatelliteData[]>([]);


onMounted(() => {
  console.log(jsonData);
})

const getResult = (query: string) => {
  searchedResult.value = query === ''
    ? jsonData
    : jsonData.filter((satellite) => {
      return (satellite.name?.toLowerCase().match(query.toLowerCase()) || satellite.noradCatId.match(query))
    })
};
</script>
