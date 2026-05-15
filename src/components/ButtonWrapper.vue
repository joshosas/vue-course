<!-- ButtonWrapper.vue -->

<script setup>
import { defineProps } from 'vue'
import { RouterLink } from 'vue-router'

defineProps({
  href: {
    type: String,
    default: '/jobs',
  },
  bg: {
    type: String,
    default: 'bg-black',
  },
})
</script>

<template>
  <RouterLink
    :href="`${href}`"
    :class="`inline-block ${bg} text-white rounded-lg px-4 py-2 hover:bg-gray-700`"
  >
    <slot />
  </RouterLink>
</template>
