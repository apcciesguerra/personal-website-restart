<script setup lang="ts">
import { computed, ref, watch, onMounted, onUnmounted } from 'vue'
import { useColorMode, useIntersectionObserver } from '@vueuse/core'
import ParticlesBg from './components/ParticlesBg.vue'
import TextHighlight from './components/TextHighlight.vue'
import Timeline from './components/Timeline.vue'
import HyperText from './components/HyperText.vue'
import NumberTicker from './components/NumberTicker.vue'
import BlurReveal from './components/BlurReveal.vue'
import ExpandableGallery from './components/ExpandableGallery.vue'
import VanishingInput from './components/VanishingInput.vue'
import RotatingPlaceholderInput from './components/RotatingPlaceholderInput.vue'
import LinkPreview from './components/LinkPreview.vue'
import { supabase } from './lib/supabase'

// Import images from assets folder
import photo1 from './assets/images/photo1.jpg'
import photo2 from './assets/images/photo2.jpg'
import photo3 from './assets/images/photo3.jpg'
import photo4 from './assets/images/photo4.jpg'
import photo5 from './assets/images/photo5.jpg'

// Last page text input
const lastPageText = ref("");
const visitorName = ref("");
const currentNamePlaceholder = ref(0);
const namePlaceholders = [
  "Your awesome name here",
  "John Doe",
  "Mary Poppins",
  "Jamal Palconan",
];
const lastPagePlaceholders = [
  "I like your website!",
  "Nah, it's ugly dude.",
  "Cool site!",
  "Amogus",
];

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
const aboutMeSection = ref<HTMLElement | null>(null)
const isAboutMeVisible = ref(false)

// Gallery images
const galleryImages = [
  photo1,
  photo2,
  photo3,
  photo4,
  photo5
];

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

// Timeline content
const timelineHeadings = [
  "Discovered Programming",
  "Graduated Junior High School",
  "IT Bootcamp in Senior High School",
  "Graduated Senior Highschool",
  "Present Day!"
];

const timelineDescriptions = [
  "First introduction to coding via HTML in 7th Grade",
  "Learned basics of HTML & CSS for 4 years (7th-10th Grade)",
  "Decided to go to a IT related bootcamp at end of Senior High",
  "Finished Senior High with more knowledge in coding",
  "Currently in my 2nd year in Computer Science"
];

useIntersectionObserver(introSection, ([{ isIntersecting }]) => {
  isIntroVisible.value = isIntersecting
}, {
  threshold: 0.2
})

useIntersectionObserver(aboutMeSection, ([{ isIntersecting }]) => {
  isAboutMeVisible.value = isIntersecting
}, {
  threshold: 0.2
})

// Rotate name placeholders
let nameIntervalId: number | null = null;

onMounted(() => {
  // Force scroll to top on page load
  window.history.scrollRestoration = 'manual';
  window.scrollTo(0, 0);
  
  // Start rotating name placeholders
  nameIntervalId = window.setInterval(() => {
    currentNamePlaceholder.value = (currentNamePlaceholder.value + 1) % namePlaceholders.length;
  }, 4500);
});

onUnmounted(() => {
  // Clear interval when component is unmounted
  if (nameIntervalId !== null) {
    clearInterval(nameIntervalId);
  }
});
// Form submission state
const isSubmitting = ref(false);
const submissionStatus = ref('');
const isError = ref(false);

// Function to submit feedback to Supabase
async function submitFeedback() {
  if (isSubmitting.value) return;
  
  if (!visitorName.value || !lastPageText.value) {
    submissionStatus.value = "Please fill in both fields";
    isError.value = true;
    return;
  }
  
  isSubmitting.value = true;
  submissionStatus.value = 'Submitting...';
  isError.value = false;
  
  try {
    console.log('Submitting feedback with data:', {
      name: visitorName.value,
      comments: lastPageText.value,
      important: true
    });
    
    // Submit to Supabase
    const { data, error } = await supabase
      .from('comments')
      .insert([
        { 
          name: visitorName.value, 
          comments: lastPageText.value,
          important: true
        }
      ]);
      
    if (error) {
      console.error('Error submitting feedback:', error);
      throw error;
    }
    
    console.log('Submission successful:', data);
    submissionStatus.value = 'Thank you for your feedback!';
    
    // Reset form
    visitorName.value = "";
    lastPageText.value = "";
    
  } catch (error) {
    console.error('Failed to submit feedback:', error);
    isError.value = true;
    submissionStatus.value = `Error: ${error.message || 'Failed to submit feedback'}`;
  } finally {
    isSubmitting.value = false;
    
    // Clear status message after 5 seconds
    setTimeout(() => {
      submissionStatus.value = '';
    }, 5000);
  }
}
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
          </TextHighlight><br>
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
              {{ timelineHeadings[index] }}
            </h3>
          </div>
          <p class="text-neutral-800 dark:text-neutral-200 text-xs md:text-sm font-normal mb-8">
            {{ timelineDescriptions[index] }}
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
        <span class="pointer-events-none text-center text-8xl font-semibold leading-none"> ☀️ </span>
        <!-- Inner Circles -->
        <Orbit
          class="size-[50px] items-center justify-center border-none bg-transparent"
          :duration="20"
          :delay="20"
          :radius="80"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <HtmlIcon />
        </Orbit>
        <Orbit
          class="size-[50px] items-center justify-center border-none bg-transparent"
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
          class="size-[50px] items-center justify-center border-none bg-transparent"
          :radius="205"
          :duration="20"
          path
        >
          <JavaScriptIcon />
        </Orbit>
        <Orbit
          class="size-[50px] items-center justify-center border-none bg-transparent"
          :radius="250"
          :duration="20"
          :delay="15"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <PythonIcon />
        </Orbit>
        <Orbit
          class="size-[50px] items-center justify-center border-none bg-transparent"
          :radius="160"
          :delay="4"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <MySqlIcon />
        </Orbit>
        <Orbit
          class="size-[50px] items-center justify-center border-none bg-transparent"
          :radius="120"
          :duration="15"
          :delay="10"
          path
        >
          <VueIcon />
        </Orbit>
      </div>
    </div>

    <!-- More About Me Section -->
    <div ref="aboutMeSection" class="py-20 min-h-screen flex flex-col items-center justify-center">
      <HyperText
        text="More About Me"
        class="text-4xl font-bold mb-16"
        :duration="2000"
      />
      
      <div class="text-center max-w-2xl mx-auto space-y-12">
        <div class="text-xl">
          I used to play video games a lot.
          <br/>
          Like, <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">a lot</TextHighlight>.
        </div>
        
        <div class="text-xl mt-8">
          There was even a time I chased being an eSports Professional
        </div>
        
        <div class="space-y-4 mt-8">
          <p class="text-lg flex items-center justify-center gap-2">
            Counter-Strike       
            <NumberTicker
              :value="3270"
              class="text-4xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
          <p class="text-lg flex items-center justify-center gap-2">
            Valorant 
            <NumberTicker
              :value="985"
              class="text-4xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
          <p class="text-lg flex items-center justify-center gap-2">
            Aimlabs 
            <NumberTicker
              :value="525"
              class="text-4xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
        </div>
        
        <div 
          class="mt-16 transition-all duration-1000 transform"
          :class="isAboutMeVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'"
        >
          <p class="text-xl">
            Nowadays, I'm just <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">pretty chill</TextHighlight> and play games for <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">fun</TextHighlight>
          </p>
          
          <div class="space-y-4 mt-8">
            <p class="text-lg flex items-center justify-center gap-2">
              Team Fortress 2 
              <NumberTicker
                :value="1011"
                class="text-4xl font-bold"
                :duration="2000"
                :decimalPlaces="0"
              /> 
              hours played
            </p>
            <p class="text-lg flex items-center justify-center gap-2">
              Deadlock
              <NumberTicker
                :value="269"
                class="text-4xl font-bold"
                :duration="2000"
                :decimalPlaces="0"
              /> 
              hours played
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- Goals Section -->
    <div class="py-20 min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-background to-background/80 relative">
      <HyperText
        text="Goals"
        class="text-4xl font-bold mb-16"
        :duration="2000"
      />
      
      <div class="container mx-auto px-4">
        <BlurReveal class="space-y-6 text-xl max-w-2xl mx-auto" whileInView>
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>Create <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">web app products</TextHighlight> independently <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">within the year</TextHighlight></span>
          </div>
          
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>Learn <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">C++</TextHighlight></span>
          </div>
          
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>Learn <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">GameDev</TextHighlight></span>
          </div>
          
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>Study and practice coding</span>
          </div>
          
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>Stay consistent with the <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">gym</TextHighlight></span>
          </div>
          
          <div class="flex items-center gap-3">
            <span>-</span>
            <span>and <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">graduate, eyy 🤙</TextHighlight></span>
          </div>
        </BlurReveal>
      </div>
    </div>

    <!-- Picture Gallery Section -->
    <div class="py-20 min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-background to-background/80 relative">
      <HyperText
        text="Picture Gallery"
        class="text-4xl font-bold mb-16"
        :duration="2000"
      />
      
      <div class="container mx-auto px-4">
        <ExpandableGallery
          :images="galleryImages"
          class="p-4"
        />
      </div>
    </div>

    <!-- Last Page Section -->
    <div class="py-20 min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-background to-background/80">
      <div class="container mx-auto px-4 max-w-3xl">
        <div class="flex h-[40rem] flex-col items-center justify-center px-4">
          <h2 class="mb-10 text-center text-xl text-black sm:mb-20 sm:text-5xl dark:text-white">
            How'd you like my website?
          </h2>
          
          <!-- Form with submission status -->
          <form @submit.prevent="submitFeedback" class="w-full max-w-xl">
            <!-- Name Input Field -->
            <div class="w-full max-w-xl mb-6">
              <RotatingPlaceholderInput
                v-model="visitorName"
                :placeholders="namePlaceholders"
              />
            </div>
            
            <VanishingInput
              v-model="lastPageText"
              :placeholders="lastPagePlaceholders"
            />
            
            <!-- Submit button -->
            <div class="mt-6 flex flex-col items-center">
              <button 
                type="submit" 
                class="px-6 py-2 bg-gradient-to-r from-indigo-500 to-purple-500 text-white rounded-lg hover:opacity-90 transition-opacity"
                :disabled="isSubmitting"
              >
                {{ isSubmitting ? 'Sending...' : 'Send Feedback' }}
              </button>
              
              <!-- Status message -->
              <div 
                v-if="submissionStatus" 
                class="mt-4 text-center transition-opacity duration-300"
                :class="isError ? 'text-red-500 dark:text-red-400' : 'text-green-500 dark:text-green-400'"
              >
                {{ submissionStatus }}
              </div>
            </div>
          </form>
        </div>
        
        <!-- Link Preview Section -->
        <div class="flex h-[40rem] flex-col items-center justify-center px-4">
          <p class="mx-auto mb-10 max-w-3xl text-xl text-neutral-500 md:text-3xl dark:text-neutral-400">
            Links to my 
            <LinkPreview
              url="https://github.com/apcciesguerra"
              class="font-bold"
            >
              <span
                class="bg-gradient-to-br from-indigo-500 to-purple-500 bg-clip-text font-bold text-transparent"
              >
                GitHub
              </span>
            </LinkPreview>
            and
            <LinkPreview
              url="https://www.linkedin.com/in/christian-esguerra-bscs/"
              class="font-bold"
            >
              <span
                class="bg-gradient-to-br from-purple-500 to-pink-500 bg-clip-text font-bold text-transparent"
              >
                LinkedIn
              </span>
            </LinkPreview>
            if you wanna check those out
          </p>
        </div>
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