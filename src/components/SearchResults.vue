<template>
  <div class="px-4 sm:px-6 lg:px-8">
    <div v-show="itemsToDisplay.length"
      class="-mx-4 mt-8 shadow ring-1 ring-black ring-opacity-5 sm:-mx-6 md:mx-0 md:rounded-lg">
      <table class="w-full divide-y divide-gray-300">
        <tbody class="divide-y divide-gray-200 font-medium">
          <tr v-for="item in itemsToDisplay" :key="item.noradCatId">
            <td class="w-full flex gap-2 p-3 text-sm md:text-base text-primary-3 sm:pl-6">
              <div class="w-20 h-20 flex justify-center items-center border border-gray-200 rounded-lg">
                <img src="../assets/vue.svg" alt="image">
              </div>
              <div>
                <p>{{ item.name }}</p>
                <p>{{ item.noradCatId }}</p>
                <p>{{ item.orbitCode }}</p>
                <p>{{ item.objectType }}</p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- Pagination -->
      <nav class="border-t border-gray-200 px-4 mx-4 flex items-center justify-between sm:px-0 h-16">
        <div class="flex-1">
          <button type="button" href="#" :class="[isLeftArrowShow ? '' : 'opacity-50',
            'pt-4 pr-1 inline-flex items-center text-gray-700 text-sm font-medium']"
            @click="goToPage(currentPageNo - 1)">
            <ArrowLeftIcon class="mr-3 h-5 text-gray-700" aria-hidden="true" />
            Previous
          </button>
        </div>
        <div class="flex items-center">
          <p v-show="currentPageNo - 1 > 0" class="text-base hover:cursor-pointer hover:bg-gray-200 px-3 py-1"
            @click="goToPage(currentPageNo - 1)">{{
              currentPageNo - 1 }}
          </p>
          <p class="border border-gray-200 text-lg px-4">{{ currentPageNo }}</p>
          <p class="text-base hover:cursor-pointer px-3 py-1 hover:bg-gray-200" @click=" goToPage(currentPageNo + 1)"
            v-show="currentPageNo + 1 < currentSearchedItems.length / 10">{{ currentPageNo + 1 }}</p>
        </div>
        <div class="flex-1 flex justify-end">
          <button type="button" href="#" :class="[isRightArrowShow ? '' : 'opacity-50',
            'pt-4 pr-1 inline-flex items-center text-gray-700 text-sm font-medium']"
            @click="goToPage(currentPageNo + 1)">
            Next
            <ArrowRightIcon class="ml-3 h-5 text-gray-700" aria-hidden="true" />
          </button>
        </div>
      </nav>
    </div>
  </div>
</template>
<script setup lang="ts">
import { PropType, watch, ref, onMounted, computed } from 'vue';
import { ArrowLeftIcon, ArrowRightIcon } from '@heroicons/vue/24/solid'

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

const props = defineProps({

  searchResults: {
    type: Array as PropType<SatelliteData[]>,
    default: () => [],
  }
})

const currentSearchedItems = ref<SatelliteData[]>([]);
const itemsToDisplay = ref<SatelliteData[]>([]);
const currentPageNo = ref(1);

const isLeftArrowShow = computed(() => currentPageNo.value > 1 && currentPageNo.value < currentSearchedItems.value.length / 10);
const isRightArrowShow = computed(() => currentPageNo.value + 1 < currentSearchedItems.value.length / 10);

watch(() => props.searchResults, (results) => {
  currentSearchedItems.value = results;
  itemsToDisplay.value = results.slice(0, 10);
})

const goToPage = (pageNumber: number) => {
  if (pageNumber <= 0) currentPageNo.value = 1;
  else if (pageNumber >= (currentSearchedItems.value.length / 10)) currentPageNo.value = Math.floor(currentSearchedItems.value.length / 10);
  else currentPageNo.value = pageNumber
  const items = currentPageNo.value * 10
  itemsToDisplay.value = currentSearchedItems.value.slice(items, items + 10)
}

onMounted(() => {
  currentSearchedItems.value = props.searchResults;
  itemsToDisplay.value = props.searchResults.slice(0, 10);
})
</script>