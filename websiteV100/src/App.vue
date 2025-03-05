<script setup>
import TextHighlight from './components/TextHighlight.vue';
</script>

<h1>test</h1>

<template>
  <div class="text-blance flex justify-center p-10">
    <h1 class="text-balance text-center text-4xl font-bold">
      You are a
      <TextHighlight class="bg-gradient-to-r from-indigo-300 to-purple-300">
        f****ing great developer!
      </TextHighlight>
    </h1>
  </div>
</template>

<style>
@import './style.css';
@import './output.css';
</style>