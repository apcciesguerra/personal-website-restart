<template>
  <div :class="cn('flex h-96 w-full gap-2', props.class)">
    <div
      v-for="image in images"
      :key="image"
      :class="[
        'relative flex h-full flex-1 cursor-pointer overflow-hidden rounded-xl transition-all duration-500 ease-in-out',
        isPhoto4(image) ? 'hover:flex-[1.5]' : 'hover:flex-[3]'
      ]"
    >
      <img
        class="relative h-full w-full object-cover"
        :src="image"
        :alt="image"
        :style="{ objectPosition: 'center' }"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import type { HTMLAttributes } from "vue";
import { cn } from "../lib/utils";

interface Props {
  images: string[];
  class?: HTMLAttributes["class"];
}

const props = defineProps<Props>();

// Function to check if the image is photo4.jpg
const isPhoto4 = (imageSrc: string) => {
  return imageSrc.includes('photo4.jpg');
};
</script>