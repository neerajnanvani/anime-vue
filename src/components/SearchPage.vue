<template>
  <div class="h-full space-y-10 bg-gradient-to-r from-cyan-100/30 to-blue-100/30  ">
    <div class="border-b-2 border-blue-400 py-10 bg-center bg-cover bg-opacity-10"
      style="background-image: url('https://media.b2broker.com/app/uploads/2021/12/b2brokerb2prime.png')">
      <h1 class="w-full text-center text-4xl my-10">Search Anime Characters</h1>
      <div class="my-6">
        <div class="flex flex-col items-center space-y-6">
          <SearchInput :modelValue="searchText" @update:modelValue="newValue => searchText = newValue" />
          <p class="mb-10 text-xl font-extrabold shadow-xl">
            Total {{ allAnimeCount }} matching anime charatcers found
          </p>
        </div>
      </div>
    </div>
    <div v-if="!animeLoaded">
      <div class="h-32 w-full flex justify-center items-center">
        <div class="flex h-32 w-full items-center justify-center">
          <div
            class="flex h-14 w-14 items-center justify-center rounded-full bg-gradient-to-tr from-cyan-500 to-pink-500 animate-spin">
            <div class="h-9 w-9 rounded-full bg-gray-200"></div>
          </div>
        </div>
      </div>
    </div>
    <div v-else-if="allAnimeCount">
      <SingleAnime v-for="anime in searchItems" :key="anime" :anime-data="anime" class="my-5" />
      <div class="w-full flex justify-center space-x-8 mt-10 pb-10">
        <button :disabled="currentPage == 1" :class="[currentPage == 1 ? 'bg-gray-200 text-gray-700 border-1' :
            'bg-blue-100 text-blue-900 hover:border-blue-600 ', 'px-3 py-2 flex items-center rounded-md border-2']"
          @click="currentPage = currentPage - 1">
          <ChevronLeft class="w-4 mr-2" />
          Previous
        </button>
        <button :disabled="!isNextPageAvailable" :class="[isNextPageAvailable ? 'bg-blue-100 text-blue-900 hover:border-blue-600' :
            'bg-gray-200 text-gray-700 border-1', 'px-3 py-2 flex items-center rounded-md border-2']"
          @click="currentPage = currentPage + 1">
          Next
          <ChevronRight class="w-4 ml-2" />
        </button>
      </div>
    </div>
    <div v-else class="w-full h-full">
      <p class="text-3xl w-full h-96 flex justify-center items-center">
        No results Found
      </p>
    </div>
  </div>
</template>
<script setup>
import ChevronLeft from "@heroicons/vue/24/solid/ChevronLeftIcon";
import ChevronRight from "@heroicons/vue/24/solid/ChevronRightIcon";
import { ref, computed, watch, onMounted } from "vue";
import SingleAnime from "./SingleAnime.vue";
import SearchInput from "./SearchInput.vue";
import axios from "axios";

// required data variables 
const searchText = ref("");
const currentPage = ref(1);
const searchItems = ref([]);
const searchTimer = ref(null);
const animeLoaded = ref(false);
const apiResponse = ref(null);

/**
 * Funtion to fetch anime list based on search query and page number
 */
async function fetchAnimeList() {
  try {
    const response = await axios.get(
      `https://api.jikan.moe/v4/characters?page=${currentPage.value}&limit=15&q=${searchText.value}&order_by=favorites&sort=desc`
    );

    // save data in respective variables
    apiResponse.value = response.data;
    searchItems.value = apiResponse.value.data;
    animeLoaded.value = true;

  } catch (err) {
    console.log(err);
    animeLoaded.value = false;
  }
}

/**
 * Computed property to fetch anime count given by API
 */
const allAnimeCount = computed(() =>
  apiResponse.value ? apiResponse.value.pagination.items.total : 0
);

/**
 * Computed property to check is next page available for more animes
 */
const isNextPageAvailable = computed(() =>
  apiResponse.value ? apiResponse.value.pagination.has_next_page : false
);


/**
 * Watcher on search text and currentPage to run fetchAnimeList funtion 
 * There is a debouncing of 500 miliseconds
 */
watch([searchText, currentPage], ([newSearchText]) => {

  // assign 1 to current page whenever query changes
  if (newSearchText) currentPage.value = 1;

  animeLoaded.value = false;
  clearTimeout(searchTimer.value);
  searchTimer.value = setTimeout(() => {
    fetchAnimeList();
  }, 500);
});

// run fetchAnimeList function when component is mounted 
onMounted(async () => {
  await fetchAnimeList();
});
</script>
