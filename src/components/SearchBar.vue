<template>
  <div class="w-full">
    <div
      class="flex border rounded-full w-full pl-4 shadow-sm sm:text-base text-xs font-medium focus:border-gray-100 focus:outline-0">
      <input class="focus:outline-0 w-full py-3" @change="(query = ($event.target as HTMLInputElement).value)"
        placeholder="Search here" @keyup="searchQueryResult($event.code)" />
      <div class="px-3 md:px-4 lg:px-5 border-l flex items-center rounded-r-full hover:bg-gray-100"
        @click="searchQueryResult('search')">
        <MagnifyingGlassIcon class="w-6 h-6" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { MagnifyingGlassIcon } from '@heroicons/vue/20/solid'

const emits = defineEmits(['searchQueryResult']);

const query = ref('')

const searchQueryResult = (e: string) => {
  if (e === "Enter" || e === "search") emits("searchQueryResult", query);
}
</script>