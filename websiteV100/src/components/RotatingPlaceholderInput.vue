<template>
  <form
    class="relative mx-auto h-12 w-full max-w-xl overflow-hidden rounded-full bg-white shadow-[0px_2px_3px_-1px_rgba(0,0,0,0.1),_0px_1px_0px_0px_rgba(25,28,33,0.02),_0px_0px_0px_1px_rgba(25,28,33,0.08)] transition duration-200 dark:bg-zinc-800"
  >
    <!-- Text Input -->
    <input
      ref="inputRef"
      v-model="inputValue"
      type="text"
      class="relative z-50 size-full rounded-full border-none bg-transparent pl-4 pr-20 text-sm text-black focus:outline-none focus:ring-0 sm:pl-10 sm:text-base dark:text-white"
    />

    <!-- Placeholder Text -->
    <div class="pointer-events-none absolute inset-0 flex items-center rounded-full">
      <Transition
        v-show="!inputValue"
        mode="out-in"
        enter-active-class="transition duration-300 ease-out"
        leave-active-class="transition duration-300 ease-in"
        enter-from-class="opacity-0 translate-y-4"
        enter-to-class="opacity-100 translate-y-0"
        leave-from-class="opacity-100 translate-y-0"
        leave-to-class="opacity-0 -translate-y-4"
      >
        <p
          :key="currentPlaceholder"
          class="w-[calc(100%-2rem)] truncate pl-4 text-left text-sm font-normal text-neutral-500 sm:pl-10 sm:text-base dark:text-zinc-500"
        >
          {{ placeholders[currentPlaceholder] }}
        </p>
      </Transition>
    </div>
  </form>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

// Define interfaces for props
interface Props {
  placeholders: string[];
}

const inputValue = defineModel<string>({
  default: "",
});

// template refs
const inputRef = ref<HTMLInputElement | null>(null);

// normal refs
const currentPlaceholder = ref<number>(0);
const intervalRef = ref<number | null>(null);

// props - no need to import defineProps or withDefaults
const props = withDefaults(defineProps<Props>(), {
  placeholders: () => ["Placeholder 1", "Placeholder 2", "Placeholder 3"],
});

// Focus on input when mounted
// Remove or comment out the auto-focus code
onMounted(() => {
  // Auto-focus removed to prevent page scrolling to this element
  
  changePlaceholder();
  document.addEventListener("visibilitychange", handleVisibilityChange);
});

function changePlaceholder(): void {
  intervalRef.value = window.setInterval(() => {
    currentPlaceholder.value = (currentPlaceholder.value + 1) % props.placeholders.length;
  }, 4500);
}

function handleVisibilityChange(): void {
  if (document.visibilityState !== "visible" && intervalRef.value) {
    clearInterval(intervalRef.value);
    intervalRef.value = null;
  } else if (document.visibilityState === "visible") {
    changePlaceholder();
  }
}

onBeforeUnmount(() => {
  if (intervalRef.value) {
    clearInterval(intervalRef.value);
  }
  document.removeEventListener("visibilitychange", handleVisibilityChange);
});
</script>