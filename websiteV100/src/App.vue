<script setup lang="ts">
import { computed, ref } from 'vue'
import { useColorMode, useIntersectionObserver } from '@vueuse/core'
import ParticlesBg from './components/ParticlesBg.vue'
import TextHighlight from './components/TextHighlight.vue'
import Timeline from './components/Timeline.vue'

const isDark = computed(() => useColorMode().value === "dark")
const introSection = ref<HTMLElement | null>(null)
const isIntroVisible = ref(false)

// Timeline data
const data = [
  {
    id: "version1.0",
    label: "Version 1.0",
  },
  {
    id: "version2.0",
    label: "Version 2.0",
  },
  {
    id: "version3.0",
    label: "Version 3.0",
  },
  {
    id: "version4.0",
    label: "Version 4.0",
  },
  {
    id: "version5.0",
    label: "Version 5.0",
  },
  {
    id: "version6.0",
    label: "Version 6.0",
  },
];

useIntersectionObserver(introSection, ([{ isIntersecting }]) => {
  isIntroVisible.value = isIntersecting
}, {
  threshold: 0.2
})
</script>

<template>
  <main class="min-h-screen w-full">
    <!-- Hero Section with Particles -->
    <div
      class="relative flex h-screen w-full flex-col items-center justify-center overflow-hidden bg-background"
    >
      <span
        class="pointer-events-none relative z-10 whitespace-pre-wrap bg-gradient-to-b from-black to-gray-300/80 bg-clip-text text-center text-8xl font-semibold leading-none text-transparent dark:from-white dark:to-slate-900/10"
      >
        Welcome To My Website
      </span>
      <ParticlesBg
        class="absolute inset-0"
        :quantity="100"
        :ease="100"
        :color="isDark ? '#FFF' : '#000'"
        :staticity="10"
      />
    </div>

    <!-- Introduction Section -->
    <div
      ref="introSection"
      class="relative flex min-h-screen w-full flex-col items-center justify-center"
    >
      <div 
        class="transition-all duration-1000"
        :class="isIntroVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'"
      >
        <h1 class="text-balance text-center text-4xl font-bold">
             Hi, I'm
          <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">
            Christian
          </TextHighlight>
            Luis Esguerra, <br>
            a
          <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">
           2nd Year College Student
          </TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300"><br>
            currently studying BS
          <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">
            Computer Science
          </TextHighlight>
        </h1>
      </div>
    </div>
    
    <!-- Timeline Section -->
    <div class="h-fit w-full">
      <Timeline
        :items="data"
        title="Beam me up"
        description="Show the timeline in style"
      >
        <template
          v-for="(item, index) in data"
          :key="item.id + 'template'"
          #[item.id]
        >
          <div class="relative w-full pl-20 pr-4 md:pl-4">
            <h3
              class="mb-4 block text-left text-2xl font-bold text-neutral-500 dark:text-neutral-500"
            >
              {{ `Section ${index + 1}` }}
            </h3>
          </div>
          <p class="text-neutral-800 dark:text-neutral-200 text-xs md:text-sm font-normal mb-8">
            Inspira UI gives you the freedom to design awesome website, in less time.
          </p>
        </template>
      </Timeline>
    </div>
  </main>
</template>

<style>
@import './style.css';

html {
  scroll-behavior: smooth;
}
</style>