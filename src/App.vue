<!-- App.vue -->

<script setup>
import Hero from './components/Hero.vue'
import Navbar from './components/Navbar.vue'
import HomeCards from './components/HomeCards.vue'
import JobListings from './components/JobListings.vue'
</script>

<template>
  <Navbar />
  <Hero title="E concern U" />
  <HomeCards />
  <JobListings :limit="3" , showButton="true" />
</template>
