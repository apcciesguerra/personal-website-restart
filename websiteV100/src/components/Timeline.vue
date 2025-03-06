<script lang="ts" setup>
import { Motion, useScroll, useTransform } from "motion-v";
import { ref, watch, onMounted, nextTick } from 'vue';
import type { HTMLAttributes } from "vue";

interface Props {
    containerClass?: HTMLAttributes["class"];
    class?: HTMLAttributes["class"];
    items: {
      id: string;
      label: string;
    }[];
    title?: string;
    description?: string;
  }
  
  const props = withDefaults(defineProps<Props>(), {
    items: () => [],
  });
  
  const timelineContainerRef = ref<HTMLElement | null>(null);
  const timelineRef = ref<HTMLElement | null>(null);
  const height = ref(0);
  
  onMounted(async () => {
    await nextTick();
    if (timelineRef.value) {
      const rect = timelineRef.value.getBoundingClientRect();
      height.value = rect.height;
    }
  });
  
  const { scrollYProgress } = useScroll({
    target: timelineRef,
    offset: ["start 10%", "end 50%"],
  });
  
  const opacityTransform = useTransform(scrollYProgress, [0, 0.1], [0, 1]);
  // Modify how heightTransform is calculated
  let heightTransform = useTransform(scrollYProgress, [0, 1], [0, 0]);
  
  watch(height, (newHeight) => {
    // Adjust the transform range to ensure it goes all the way
    heightTransform = useTransform(scrollYProgress, [0, 0.9], [0, newHeight]);
  });
</script>

<template>
  <div
    ref="timelineContainerRef"
    class="w-full font-sans md:px-10 backdrop-blur-sm bg-white/5 dark:bg-black/5 rounded-xl"
  >
    <div class="mx-auto max-w-7xl px-4 py-12 lg:px-10 md:px-8">
      <h2 class="mb-4 max-w-4xl text-lg text-black md:text-4xl dark:text-white">
        {{ title }}
      </h2>
      <p class="max-w-sm text-sm text-neutral-700 md:text-base dark:text-neutral-300">
        {{ description }}
      </p>
    </div>
  
    <div
      ref="timelineRef"
      class="relative mx-auto max-w-7xl pb-20"
    >
      <div
        v-for="(item, index) in props.items"
        :key="item.id + index"
        class="flex justify-start pt-10 md:gap-10 md:pt-40"
      >
        <div
          class="sticky flex flex-col md:flex-row z-40 items-center top-40 self-start max-w-xs lg:max-w-sm md:w-full"
        >
          <div
            class="h-10 absolute left-3 md:left-3 w-10 rounded-full bg-white/20 dark:bg-black/20 backdrop-blur-sm flex items-center justify-center"
          >
            <div
              class="h-4 w-4 rounded-full bg-gradient-to-r from-indigo-300 to-purple-300 border border-indigo-400 dark:border-purple-400 p-2"
            />
          </div>
          <h3
            class="hidden md:block text-xl md:pl-20 md:text-5xl font-bold bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent"
          >
            {{ item.label }}
          </h3>
        </div>
        <slot :name="item.id"></slot>
      </div>
      <div
        :style="{
          height: height + 'px',
        }"
        class="absolute left-8 top-0 w-[2px] overflow-hidden bg-[linear-gradient(to_bottom,var(--tw-gradient-stops))] from-transparent from-0% via-neutral-200 to-transparent to-[99%] [mask-image:linear-gradient(to_bottom,transparent_0%,black_10%,black_90%,transparent_100%)] md:left-8 dark:via-neutral-700"
      >
        <Motion
          as="div"
          :style="{
            height: heightTransform,
            opacity: opacityTransform,
          }"
          class="absolute inset-x-0 top-0 w-[2px] rounded-full bg-gradient-to-r from-indigo-500 from-0% via-purple-500 via-50% to-transparent"
        >
        </Motion>
      </div>
    </div>
  </div>
</template>