<template>
  <Combobox v-model="selectedPerson">
    <div class="w-full">
      <div
        class="flex border rounded-full w-full pl-4 shadow-sm sm:text-base text-xs font-medium focus:border-gray-100 focus:outline-0">
        <ComboboxInput class="focus:outline-0 w-full py-3" @change="query = $event.target.value"
          :displayValue="(person: any) => person.name" />
        <ComboboxButton class="px-3 md:px-4 lg:px-5 border-l flex items-center rounded-r-full hover:bg-gray-100">
          <MagnifyingGlassIcon class="w-6 h-6" />
        </ComboboxButton>
      </div>
      <div>
        <ComboboxOptions>
          <!-- Use the `active` state to conditionally style the active option. -->
          <!-- Use the `selected` state to conditionally style the selected option. -->
          <ComboboxOption v-for="person in filteredPeople" :key="person.id" :value="person" as="template"
            v-slot="{ active, selected }">
            <li :class="{
              'bg-blue-500 text-white': active,
              'bg-white text-black': !active,
            }"
            class="flex"
            >
              <CheckIcon v-show="selected" class="h-5 w-5" />
              {{ person.name }}
            </li>
          </ComboboxOption>
        </ComboboxOptions>
      </div>
    </div>
  </Combobox>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import {
  Combobox,
  ComboboxInput,
  ComboboxOptions,
  ComboboxOption,
  ComboboxButton
} from '@headlessui/vue'
import { CheckIcon, MagnifyingGlassIcon } from '@heroicons/vue/20/solid'

const people = [
  { id: 1, name: 'Durward Reynolds' },
  { id: 2, name: 'Kenton Towne' },
  { id: 3, name: 'Therese Wunsch' },
  { id: 4, name: 'Benedict Kessler' },
  { id: 5, name: 'Katelyn Rohan' },
]
const selectedPerson = ref(people[0])
const query = ref('')

const filteredPeople = computed(() =>
  query.value === ''
    ? people
    : people.filter((person) => {
      return person.name.toLowerCase().includes(query.value.toLowerCase())
    })
)
</script>