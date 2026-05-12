<!-- ButtonWrapper.vue -->

<script setup>
import { defineProps } from 'vue'

defineProps({
  href: {
    type: String,
    default: 'jobs.html',
  },
  bg: {
    type: String,
    default: 'bg-black',
  },
})
</script>

<template>
  <a
    :href="`${href}`"
    :class="`inline-block ${bg} text-white rounded-lg px-4 py-2 hover:bg-gray-700`"
  >
    <slot />
  </a>
</template>
