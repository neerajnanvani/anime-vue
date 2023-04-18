<template>
    <h1 class="w-full text-center text-4xl my-10">Search Anime Characters</h1>
    <div class="flex justify-center">

        <div class="mt-1 relative rounded-md shadow-sm w-1/2">
            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <MagnifyingGlass class="h-5 w-5 text-gray-600" aria-hidden="true" /> 
                <span class="mx-2 text-gray-500"> | </span>
            </div>
            <input v-model="searchText" name="search" type="text" class="focus:ring-indigo-500 focus:border-indigo-500 block w-full pl-14 sm:text-sm border-gray-500 rounded-2xl" />
        </div>
    </div>

</template>
<script setup>

import MagnifyingGlass from '@heroicons/vue/24/solid/MagnifyingGlassIcon';
import {ref, computed, watch} from "vue";

const searchText = ref("");
const currentPage = ref(1);
const searchItems = ref([]);
const searchTimer = ref(null);

async function fetchAnimes() {
    const res = await fetch(`https://api.jikan.moe/v4/characters?page=${currentPage.value}&limit=15&q=${searchText.value}&order_by=favorites&sort=desc`);
    const finalRes = await res.json();
    searchItems.value = finalRes.data;
    console.log({finalRes, res});
    console.log(searchItems.value)
}

watch(searchText, () => {
    clearTimeout(searchTimer.value);
    searchTimer.value = setTimeout(() => {
      fetchAnimes();
    }, 500);
});

</script>