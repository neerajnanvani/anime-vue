<template>
  <div>
    <h1 class="w-full text-center text-4xl my-6">Search Anime Characters</h1>
    <div class="my-6">
      <div class="flex flex-col items-center space-y-6">
        <div class="mt-1 relative rounded-md shadow-sm w-1/2">
          <div
            class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
          >
            <MagnifyingGlass class="h-5 w-5 text-gray-600" aria-hidden="true" />
            <span class="mx-2 text-gray-500"> | </span>
          </div>
          <input
            v-model="searchText"
            name="search"
            type="text"
            autocomplete="off"
            class="focus:ring-indigo-500 focus:border-indigo-500 block w-full pl-14 sm:text-sm border-gray-500 rounded-2xl"
          />
        </div>

        <p>Total {{ allAnimeCount }} matching anime charatcers found</p>
      </div>

      <hr class="border border-gray-500 mt-6" />
    </div>
    <SingleAnime
      v-for="anime in searchItems"
      :key="anime"
      :anime-data="anime"
      class="my-3"
    />
    <div class="w-full flex justify-center space-x-8 mt-10 mb-20">
      <button
        :class="[currentPage == 1 ?  'bg-gray-200 text-gray-700 border-1' : 'bg-blue-100 text-blue-900 hover:border-blue-600 ', 'px-3 py-2 flex items-center rounded-md border-2']"
      >
        <ChevronLeft class="w-4 mr-2"/> Previous
      </button>
      <button
        :class="[isNextPageAvailable ? 'bg-blue-100 text-blue-900  hover:border-blue-600' : 'bg-gray-200 text-gray-700 border-1', 'px-3 py-2 flex items-center rounded-md border-2']"
      >
        Next <ChevronRight class="w-4 ml-2" />
      </button>
    </div>
  </div>
</template>
<script setup>
import MagnifyingGlass from "@heroicons/vue/24/solid/MagnifyingGlassIcon";
import ChevronLeft from "@heroicons/vue/24/solid/ChevronLeftIcon";
import ChevronRight from "@heroicons/vue/24/solid/ChevronRightIcon";
import { ref, computed, watch, onMounted } from "vue";
import SingleAnime from "./SingleAnime.vue";
import axios from "axios";

const searchText = ref("");
const currentPage = ref(1);
const searchItems = ref([]);
const searchTimer = ref(null);
const animeLoaded = ref(false);

const apiResponse = ref(null);

async function fetchAnimes() {
  try {
    const response = await axios.get(
      `https://api.jikan.moe/v4/characters?page=${currentPage.value}&limit=15&q=${searchText.value}&order_by=favorites&sort=desc`
    );

    apiResponse.value = { ...response.data };
    searchItems.value = apiResponse.value.data;
    animeLoaded.value = true;

    console.log(apiResponse.value)
  } catch (err) {
    console.log(err);
    animeLoaded.value = false;
  }
}


const allAnimeCount = computed(() =>
  apiResponse.value ? apiResponse.value.pagination.items.total : 0
);

const isNextPageAvailable = computed(() =>
  apiResponse.value ? apiResponse.value.pagination.current_page : false
);


watch([searchText, currentPage], () => {
  animeLoaded.value = false;
  clearTimeout(searchTimer.value);
  searchTimer.value = setTimeout(() => {
    fetchAnimes();
  }, 500);
});

onMounted(async () => {
  await fetchAnimes();
});
</script>
