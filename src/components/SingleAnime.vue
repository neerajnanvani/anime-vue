<template>
  <div class="mx-20">
    <div class="w-full border-2 border-gray-500 flex flex-row h-20">
      <div class="w-20">
        <img
          :src="animeData.images.webp.image_url"
          class="h-full w-full p-2"
        />
      </div>
      <div class="flex flex-col grow justify-center space-y-3">
        <p>{{animeData.name}}</p>
        <div class="flex flex-wrap space-x-1">
          <span
          v-for="nickname in animeData.nicknames"
            :key="nickname"
            class="inline-flex items-center px-2.5 py-0.5 rounded-md text-xs font-medium bg-gray-100 text-blue-800 border border-blue-800"
          >
            {{ nickname }} 
          </span>
        </div>
      </div>
      <div class="flex justify-center my-2 mx-4">
          <Heart class="text-red-600 w-4 self-start mt-1 " /> 
          {{animeData.favorites}}
        </div>
      <a :href="animeData.url" class="border-l-2 w-20 "><ArrowRightCircle class="w-10 mx-5 my-5 text-blue-800"/> </a>
    </div>
  </div>
</template>

<script setup>
import Heart from '@heroicons/vue/24/solid/HeartIcon';
import ArrowRightCircle from '@heroicons/vue/24/solid/ArrowSmallRightIcon';
import { defineProps } from 'vue';

defineProps({
    animeData : {
        type: Object,
        required: true,
    }
})

</script>
