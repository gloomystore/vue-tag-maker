<template>
  <div id="app">
    <GloomyTags
      :state="tags"
      :setState="updateTags"
      name="tags"
      placeHolder="Type and press enter"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
// import GloomyTags from './libs/components/GloomyTags.vue'
import GloomyTags from './dist'

const tags = ref<string[]>([])

function updateTags(newTags: string[]) {
  tags.value = newTags
}
</script>
