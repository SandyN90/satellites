<template>
    <Combobox v-model="selectedfilter">
        <div class="block">
            <ComboboxButton>
                <div class="border border-gray-200 rounded-full flex px-3 py-3">
                    <ComboboxInput @change="query = $event.target.value" class="focus:outline-0" />
                    <ChevronUpDownIcon class="w-5 h-5" />
                </div>
            </ComboboxButton>

            <ComboboxOptions class="border border-gray-200 rounded-lg pl-2">
                <ComboboxOption v-for="item in filteredPeople" :key="item" :value="filterTypes[item]" @click="filterChanged(item)">
                    {{ filterTypes[item] }}
                </ComboboxOption>
            </ComboboxOptions>

        </div>
    </Combobox>
</template>
  
<script setup lang="ts">
import { ref, computed, PropType, watch } from 'vue'
import {
    Combobox,
    ComboboxInput,
    ComboboxOptions,
    ComboboxOption,
    ComboboxButton
} from '@headlessui/vue'
import { ChevronUpDownIcon } from '@heroicons/vue/20/solid';

const props = defineProps({
    filters: {
        type: Object as PropType<Record<string, string>>,
        default: () => {}
    }
})

const emit = defineEmits(["selectFilter"])

const filterTypes = ref<Record<string, string>>({
    "countryCode": "Country Code",
    "orbitCode": "Orbit Code",
    "objectType": "Object Type"
});
const selectedfilter = ref(filterTypes.value["countryCode"]);
const query = ref('');

watch(() => props.filters, (value) => {
    filterTypes.value = value;
})

const filteredPeople = computed(() =>
    query.value === ''
        ? Object.keys(filterTypes.value)
        : Object.keys(filterTypes.value).filter((item) => {
            return item.toLowerCase().includes(query.value.toLowerCase())
        })
)

const filterChanged = (item: string) => {
    emit("selectFilter", item);
}
</script>