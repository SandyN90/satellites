<template>
  <!-- {{ currentSearchedItems }} -->
  <div class="px-4 sm:px-6 lg:px-8">
    <div v-if="currentSearchedItems.length"
      class="-mx-4 mt-8 shadow ring-1 ring-black ring-opacity-5 dark:ring-dark-primary-3 sm:-mx-6 md:mx-0 md:rounded-lg">
      <table class="w-full divide-y divide-gray-300 dark:divide-dark-primary-3">
        <tbody class="divide-y divide-gray-200 dark:divide-dark-primary-3 dark:bg-dark-primary-2 font-medium">
          <tr v-for="item in currentSearchedItems" :key="item.noradCatId">
            <td
              class="w-full flex gap-2 p-3 text-sm md:text-base text-primary-3 dark:text-dark-text-1 hover:dark:text-gray-100 sm:pl-6">
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
            <td class="p-3 px-2 md:px-4 text-sm text-gray-600">
              <div class="flex items-center">
                <p class="flex gap-x-1 dark:text-grey-5">
                  <span class="hidden xl:block truncate">{{ (`lastUpdated`) }}</span>
                </p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- Pagination -->
      <nav class="border-t border-gray-200 dark:border-grey-6 px-4 mx-4 flex items-center justify-between sm:px-0 h-16">
        <div class="flex-1">
          <button type="button" href="#"
            :class="[isLeftArrowShow ? 'opacity-50 dark:opacity-70' : '',
              'pt-4 pr-1 inline-flex items-center text-gray-700 dark:text-grey-5 text-sm font-medium dark:text-grey-4']"
            @click="goToPage(currentPageNo - 1)">
            <ArrowLeftIcon class="mr-3 h-5 text-gray-700 dark:text-grey-4" aria-hidden="true" />
            Previous
          </button>
        </div>
        <div class="flex-1 flex justify-end">
          <button type="button" href="#"
            :class="[isRightArrowShow ? 'opacity-50 dark:opacity-70' : '',
              'pt-4 pr-1 inline-flex items-center text-gray-700 dark:text-grey-5 text-sm font-medium dark:text-grey-4']"
            @click="goToPage(currentPageNo + 1)">
            Next
            <ArrowRightIcon class="ml-3 h-5 text-gray-700 dark:text-grey-4" aria-hidden="true" />
          </button>
        </div>
      </nav>
    </div>
  </div>
</template>
<script setup lang="ts">
import { PropType, watch, ref, onMounted } from 'vue';
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
const isLeftArrowShow = ref(true);
const isRightArrowShow = ref(true);
const currentPageNo = ref(1);

watch(() => props.searchResults, (results) => {
  currentSearchedItems.value = results;
})

const goToPage = (pageNumber: number) => {
  currentPageNo.value = pageNumber;
}

onMounted(() => {
  currentSearchedItems.value = props.searchResults;
})
</script>