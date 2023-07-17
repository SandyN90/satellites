
<template>
  <div class="flex justify-center max-w-lg sm:max-w-xl md:max-w-3xl lg:max-w-5xl mt-5 sm:mt-10 lg:mt-20 mx-auto">
    <SearchBar />
  </div>
</template>

<script setup lang="ts">
import SearchBar from './SearchBar.vue';

defineProps<{ msg: string }>()
</script>
<style scoped>
.read-the-docs {
  color: #888;
}
</style>
