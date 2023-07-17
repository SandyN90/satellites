<template>
    <div
      wire:loading
      class="relative flex flex-col items-center justify-center gap-y-4"
    >
      <!-- ? Circular Loader Reference: https://larainfo.com/blogs/tailwind-css-loading-spinner-example -->
      <div
        :class="`w-12 h-12 border-4 border-t-gray-200 border-blue-400 rounded-full animate-spin`"
      />
      <div
        v-show="showInfo"
        class="mx-auto text-center text-lg"
      >
        <h2 class="text-gray-500">
          {{ infoTitle }}
        </h2>
        <h3 class="text-gray-700">
          {{ infoText }}
        </h3>
      </div>
    </div>
  </template>

<script setup lang="ts">

defineProps({
  /**
   * Prop to denote whether to show text under loader or not
   */
  showInfo: {
    type: Boolean,
    default: true,
  },
  /**
   * Prop for text title to be shown under loader
   */
  infoTitle: {
    type: String,
    default: "Loading...",
  },
  /**
   * Prop for text subtitle to be shown under loader
   */
  infoText: {
    type: String,
    default: "This may take a few seconds, please wait....",
  },
});

</script>
