<template>
  <div class="mx-20">
    <div class="w-full border-2 border-blue-300 flex flex-row h-20 rounded-md">
      <div class="w-20">
        <img :src="animeData.images.webp.image_url" class="h-full w-full p-2" />
      </div>
      <div class="flex flex-col grow justify-center space-y-3">
        <p class="ftext-gray-700 text-xl">{{ animeData.name }}</p>
        <div class="flex flex-wrap overflow-hidden space-x-1">
          <span v-for="nickname in animeData.nicknames" :key="nickname"
            class="inline-flex items-center px-2.5 py-0.5 rounded-md text-xs font-medium bg-cyan-100/20 text-blue-700 border border-blue-500">
            {{ nickname }}
          </span>
        </div>
      </div>
      <div class="flex justify-center my-2 mx-4">
        <Heart class="text-red-600 w-4 self-start mt-1 mr-2" />
        <p class="text-base text-gray-700">
          {{ animeData.favorites }}
        </p>
      </div>
      <a :href="animeData.url" class="border-l-2 w-20">
        <ArrowRightCircle class="w-10 mx-5 my-5 text-blue-800" />
      </a>
    </div>
  </div>
</template>

<script setup>
import Heart from '@heroicons/vue/24/solid/HeartIcon';
import ArrowRightCircle from '@heroicons/vue/24/solid/ArrowSmallRightIcon';
import { defineProps } from 'vue';

defineProps({
  /**
   * animeData prop to show all details about single anime
   */
  animeData: {
    type: Object,
    required: true,
  }
})

</script>
