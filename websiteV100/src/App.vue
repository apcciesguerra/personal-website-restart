<script setup lang="ts">
import { computed, ref, watch, onMounted, onUnmounted, defineAsyncComponent } from 'vue'
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
import TextHoverEffect from './components/TextHoverEffect.vue'
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

// Add refs for all sections that need delayed TextHighlight effects
const introSection = ref<HTMLElement | null>(null)
const isIntroVisible = ref(false)
const aboutMeSection = ref<HTMLElement | null>(null)
const isAboutMeVisible = ref(false)
const journeySection = ref<HTMLElement | null>(null)
const isJourneyVisible = ref(false)
const experienceSection = ref<HTMLElement | null>(null)
const isExperienceVisible = ref(false)
const goalsSection = ref<HTMLElement | null>(null)
const isGoalsVisible = ref(false)
const gallerySection = ref<HTMLElement | null>(null)
const isGalleryVisible = ref(false)
const feedbackSection = ref<HTMLElement | null>(null)
const isFeedbackVisible = ref(false)

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
}, { threshold: 0.2 })
useIntersectionObserver(aboutMeSection, ([{ isIntersecting }]) => {
  isAboutMeVisible.value = isIntersecting
}, { threshold: 0.2 })
useIntersectionObserver(journeySection, ([{ isIntersecting }]) => {
  isJourneyVisible.value = isIntersecting
}, { threshold: 0.2 })
useIntersectionObserver(experienceSection, ([{ isIntersecting }]) => {
  isExperienceVisible.value = isIntersecting
}, { threshold: 0.2 })
useIntersectionObserver(goalsSection, ([{ isIntersecting }]) => {
  isGoalsVisible.value = isIntersecting
}, { threshold: 0.2 })
useIntersectionObserver(gallerySection, ([{ isIntersecting }]) => {
  isGalleryVisible.value = isIntersecting
}, { threshold: 0.2 })
useIntersectionObserver(feedbackSection, ([{ isIntersecting }]) => {
  isFeedbackVisible.value = isIntersecting
}, { threshold: 0.2 })
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
  <main class="min-h-screen w-full relative overflow-x-hidden">
    <!-- Particles background that spans the entire page -->
    <ParticlesBg
      class="fixed inset-0 z-0"
      :quantity="150"
      :ease="100"
      :color="isDark ? '#FFF' : '#000'"  
      :staticity="10"
    />
    
    <!-- Hero Section -->
    <div
      class="relative flex h-screen w-full flex-col items-center justify-center overflow-hidden"
    >
      <span
        class="pointer-events-none relative z-10 whitespace-pre-wrap bg-gradient-to-b from-black to-gray-300/80 bg-clip-text text-center text-8xl font-semibold leading-none text-transparent dark:from-white dark:to-slate-900/10"
      >
        Welcome To My Website
      </span>
    </div>
    
    <!-- Introduction Section -->
    <div
      ref="introSection"
      class="relative flex min-h-screen w-full flex-col items-center justify-center z-10"
    >
      <div 
        class="transition-all duration-1000 max-w-4xl mx-auto px-6"
        :class="isIntroVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'"
      >
        <h1 class="text-balance text-center text-5xl md:text-6xl font-bold leading-relaxed">
          Hi, I'm 
          <TextHighlight 
            v-if="isIntroVisible" 
            class="bg-gradient-to-r from-indigo-300 to-purple-300 px-2 py-1 transition-all duration-700"
          >
            Christian
          </TextHighlight>
          <span v-else class="px-2 py-1">Christian</span>
          Luis Esguerra
          <div class="my-6">
            a 
            <TextHighlight 
              v-if="isIntroVisible" 
              class="bg-gradient-to-r from-indigo-300 to-purple-300 px-2 py-1 transition-all duration-700"
            >
              2nd Year College Student
            </TextHighlight>
            <span v-else class="px-2 py-1">2nd Year College Student</span>
          </div>
          <div class="my-6">
            currently studying BS
            <TextHighlight 
              v-if="isIntroVisible" 
              class="bg-gradient-to-r from-indigo-300 to-purple-300 px-2 py-1 transition-all duration-700"
            >
              Computer Science
            </TextHighlight>
            <span v-else class="px-2 py-1">Computer Science</span>
          </div>
        </h1>
      </div>
    </div>
    
    <!-- Timeline Section -->
    <div class="py-32 min-h-screen flex flex-col items-center justify-center relative z-10">
      <HyperText
        text="My Journey"
        class="text-5xl font-bold mb-24"
        :duration="2000"
      />
      
      <div class="container mx-auto px-4">
        <Timeline
          :items="data"
          class="shadow-lg"
        >
          <template
            v-for="(item, index) in data"
            :key="item.id + 'template'"
            #[item.id]
          >
            <div class="relative w-full pl-20 pr-4 md:pl-8">
              <h3
                class="mb-6 block text-left text-2xl md:text-3xl font-bold bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent"
              >
                {{ timelineHeadings[index] }}
              </h3>
              <div class="backdrop-blur-sm bg-white/5 dark:bg-black/10 p-5 rounded-lg shadow-md hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-y-[-5px]">
                <p class="text-neutral-800 dark:text-neutral-200 text-sm md:text-lg font-normal leading-relaxed">
                  {{ timelineDescriptions[index] }}
                </p>
              </div>
            </div>
          </template>
        </Timeline>
      </div>
    </div>
    
    <!-- HyperText and Orbit Section -->
    <div class="py-32 min-h-screen flex flex-col items-center justify-center space-y-12 relative z-10">
      <HyperText
        text="IT Experience"
        class="text-5xl font-bold mb-24"
        :duration="2000"
      />
      
      <div
        class="relative flex h-[700px] w-full max-w-5xl mx-auto flex-col items-center justify-center overflow-visible rounded-xl backdrop-blur-sm bg-white/5 dark:bg-black/10 transition-all duration-500 shadow-lg"
      >
        <span class="pointer-events-none text-center text-9xl font-semibold leading-none"> ☀️ </span>
        <!-- Inner Circles -->
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :duration="20"
          :delay="20"
          :radius="120"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <HtmlIcon />
          </div>
        </Orbit>
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :duration="20"
          :delay="10"
          :radius="120"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <CssIcon />
          </div>
        </Orbit>
        <!-- Outer Circles (reverse) -->
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :radius="280"
          :duration="25"
          path
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <JavaScriptIcon />
          </div>
        </Orbit>
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :radius="320"
          :duration="30"
          :delay="15"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <PythonIcon />
          </div>
        </Orbit>
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :radius="230"
          :delay="4"
          :duration="22"
          path
          :direction="ORBIT_DIRECTION.CounterClockwise"
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <MySqlIcon />
          </div>
        </Orbit>
        <Orbit
          class="size-[60px] items-center justify-center border-none bg-transparent"
          :radius="180"
          :duration="18"
          :delay="10"
          path
        >
          <div class="p-3 rounded-full backdrop-blur-sm bg-white/10 dark:bg-black/20 shadow-md transition-all duration-300">
            <VueIcon />
          </div>
        </Orbit>
      </div>
    </div>
    
    <!-- More About Me Section -->
    <div ref="aboutMeSection" class="py-32 min-h-screen flex flex-col items-center justify-center relative z-10">
      <HyperText
        text="More About Me"
        class="text-5xl font-bold mb-24"
        :duration="2000"
      />
      
      <div class="text-center max-w-3xl mx-auto space-y-16">
        <div class="text-2xl leading-relaxed p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
          I used to play video games a lot.
          <br/>
          Like, <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">a lot</TextHighlight>.
        </div>
        
        <div class="text-2xl leading-relaxed p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
          There was even a time I chased being an eSports Professional
        </div>
        
        <div class="space-y-8 mt-12">
          <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
            Counter-Strike       
            <NumberTicker
              :value="3270"
              class="text-5xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
          <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
            Valorant 
            <NumberTicker
              :value="985"
              class="text-5xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
          <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
            Aimlabs 
            <NumberTicker
              :value="525"
              class="text-5xl font-bold"
              :duration="2000"
              :decimalPlaces="0"
            /> 
            hours played
          </p>
        </div>
        
        <div 
          class="mt-24 transition-all duration-1000 transform"
          :class="isAboutMeVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'"
        >
          <p class="text-2xl leading-relaxed p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
            Nowadays, I'm just <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">pretty chill</TextHighlight> and play games for <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">fun</TextHighlight>
          </p>
          
          <div class="space-y-8 mt-12">
            <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
              Team Fortress 2 
              <NumberTicker
                :value="1011"
                class="text-5xl font-bold"
                :duration="2000"
                :decimalPlaces="0"
              /> 
              hours played
            </p>
            <div class="space-y-8 mt-12">
            <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
              osu!
              <NumberTicker
                :value="738"
                class="text-5xl font-bold"
                :duration="2000"
                :decimalPlaces="0"
              /> 
              hours played
            </p>
            <p class="text-xl flex items-center justify-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300">
              Deadlock
              <NumberTicker
                :value="269"
                class="text-5xl font-bold"
                :duration="2000"
                :decimalPlaces="0"
              /> 
              hours played
            </p>
          </div>
        </div>
      </div>
    </div>
    </div>
    
    <!-- Goals Section -->
        <div class="py-32 min-h-screen flex flex-col items-center justify-center relative z-10">
          <HyperText
            text="Goals"
            class="text-5xl font-bold mb-24"
            :duration="2000"
          />
          
          <div class="container mx-auto px-4">
            <BlurReveal class="space-y-10 text-2xl max-w-3xl mx-auto" whileInView>
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>Create <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">web app products</TextHighlight> independently <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">within the year</TextHighlight></span>
              </div>
              
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>Learn <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">C++</TextHighlight></span>
              </div>
              
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>Learn <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">GameDev</TextHighlight></span>
              </div>
              
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>Study and practice coding</span>
              </div>
              
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>Stay consistent with the <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">gym</TextHighlight></span>
              </div>
              
              <div class="flex items-center gap-4 p-4 rounded-lg backdrop-blur-sm bg-white/5 dark:bg-black/10 hover:bg-white/10 dark:hover:bg-black/20 transition-all duration-300 transform hover:translate-x-2">
                <span class="text-3xl text-indigo-400">→</span>
                <span>and <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">graduate, eyy 🤙</TextHighlight></span>
              </div>
            </BlurReveal>
          </div>
        </div>
    
    <!-- Picture Gallery Section -->
    <div class="py-20 min-h-screen flex flex-col items-center justify-center relative z-10">
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
    <div class="py-32 min-h-screen flex flex-col items-center justify-center relative z-10">
      <HyperText
        text="Feedback"
        class="text-5xl font-bold mb-24"
        :duration="2000"
      />
      
      <div class="container mx-auto px-4 max-w-3xl">
        <div class="flex flex-col items-center justify-center px-4 mb-40">
          <h2 class="mb-24 text-center text-3xl md:text-5xl font-bold bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent leading-relaxed py-2">
            How'd you like my website?
          </h2>
          
          <!-- Form with submission status -->
          <form @submit.prevent="submitFeedback" class="w-full max-w-xl backdrop-blur-sm bg-white/5 dark:bg-black/10 p-8 rounded-xl shadow-lg">
            <!-- Name Input Field -->
            <div class="w-full max-w-xl mb-6">
              <RotatingPlaceholderInput
                v-model="visitorName"
                :placeholders="namePlaceholders"
              />
            </div>
            
            <!-- Rest of the form remains unchanged -->
            <VanishingInput
              v-model="lastPageText"
              :placeholders="lastPagePlaceholders"
            />
            
            <!-- Submit button -->
            <div class="mt-8 flex flex-col items-center">
              <button 
                type="submit" 
                class="px-6 py-3 bg-gradient-to-r from-indigo-500 to-purple-500 text-white rounded-lg hover:opacity-90 transition-opacity transform hover:scale-105 duration-300 shadow-md"
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
        <div class="flex flex-col items-center justify-center px-4 mt-16">
          <p class="mx-auto mb-10 max-w-3xl text-2xl md:text-4xl leading-relaxed text-center">
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

    <!-- Footer with hidden text -->
      <div class="py-16 flex items-center justify-center relative z-10">
        <div class="flex h-auto items-center justify-center max-lg:w-full min-md:flex-1">
          <TextHoverEffect
            class="w-[200%] min-lg:min-h-64"
            text="vercel"
          />
        </div>
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