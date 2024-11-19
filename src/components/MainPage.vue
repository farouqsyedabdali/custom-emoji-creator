<script setup>
import { ref } from 'vue'
import Renderer from './Renderer.vue'

const baseFace = ref(null)
</script>

<template>
  <div class="space-y-6">
    <EmojiPreview :baseFace="baseFace" />
    <Renderer v-model="baseFace" />
  </div>
</template>

<style scoped>
/* Scoped styles for this component */
</style>
