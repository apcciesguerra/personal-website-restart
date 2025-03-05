<script setup lang="ts">
import { computed, ref } from 'vue'
import { useColorMode, useIntersectionObserver } from '@vueuse/core'
import ParticlesBg from './components/ParticlesBg.vue'
import TextHighlight from './components/TextHighlight.vue'
import Timeline from './components/Timeline.vue'
import HyperText from './components/HyperText.vue'

import Orbit from './components/Orbit.vue'
import { ORBIT_DIRECTION } from './index'

import HtmlIcon from './components/icons/Html_Icon.vue'
import CssIcon from './components/icons/Css_Icon.vue'
import JavaScriptIcon from './components/icons/JavaScript_Icon.vue'
import PythonIcon from './components/icons/Python_Icon.vue'
import MySqlIcon from './components/icons/MySql_Icon.vue'
import VueIcon from './components/icons/Vue_Icon.vue'

const isDark = computed(() => useColorMode().value === "dark")
const introSection = ref<HTMLElement | null>(null)
const isIntroVisible = ref(false)

// Timeline data
const data = [
  {
    id: "version1.0",
    label: "2017",
  },
  {
    id: "version2.0",
    label: "2020",
  },
  {
    id: "version3.0",
    label: "2021",
  },
  {
    id: "version4.0",
    label: "2022",
  },
  {
    id: "version5.0",
    label: "2025",
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

    <!-- HyperText and Orbit Section -->
    <div class="py-20 flex flex-col items-center justify-center space-y-12">
      <HyperText
        text="IT Experience"
        class="text-4xl font-bold"
        :duration="2000"
      />
      
      <div
        class="relative flex h-[500px] w-full max-w-3xl mx-auto flex-col items-center justify-center overflow-hidden rounded-lg border bg-background md:shadow-xl"
      >
        <span class="pointer-events-none text-center text-8xl font-semibold leading-none"> 💻 </span>
    <!-- Inner Circles -->
    <Orbit
      class="size-[40px] items-center justify-center border-none bg-transparent"
      :duration="20"
      :delay="20"
      :radius="80"
      :direction="ORBIT_DIRECTION.CounterClockwise"
    >
      <HtmlIcon />
    </Orbit>
    <Orbit
      class="size-[40px] items-center justify-center border-none bg-transparent"
      :duration="20"
      :delay="10"
      :radius="80"
      path
      :direction="ORBIT_DIRECTION.CounterClockwise"
    >
      <CssIcon />
    </Orbit>
    <!-- Outer Circles (reverse) -->
    <Orbit
      class="size-[40px] items-center justify-center border-none bg-transparent"
      :radius="190"
      :duration="20"
      path
    >
      <JavaScriptIcon />
    </Orbit>
    <Orbit
      class="size-[40px] items-center justify-center border-none bg-transparent"
      :radius="190"
      :duration="20"
      :delay="15"
      :direction="ORBIT_DIRECTION.CounterClockwise"
    >
      <PythonIcon />
    </Orbit>
    <Orbit
      class="items-center justify-center border-none bg-transparent"
      :radius="140"
      :delay="4"
    >
      <MySqlIcon />
    </Orbit>
    <Orbit
      class="size-[40px] items-center justify-center border-none bg-transparent"
      :radius="120"
      :duration="25"
      :delay="10"
    >
      <VueIcon />
    </Orbit>
  </div>
</div>
  </main>
</template>

<style>
@import './style.css';

html {
  scroll-behavior: smooth;
}
</style>