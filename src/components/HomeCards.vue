<script setup>
import ButtonWrapper from './ButtonWrapper.vue'
import Card from './Card.vue'
</script>

<template>
  <!-- Developers and Employers -->
  <section class="py-4">
    <div class="container-xl lg:container m-auto">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 p-4 rounded-lg">
        <Card bg="bg-gray-100">
          <h2 class="text-2xl font-bold">For Developers</h2>
          <p class="mt-2 mb-4">Browse our Vue jobs and start your career today</p>
          <ButtonWrapper href="jobs.html" bg="bg-black"> Browse Jobs </ButtonWrapper>
        </Card>
        <Card>
          <h2 class="text-2xl font-bold">For Employers</h2>
          <p class="mt-2 mb-4">List your job to find the perfect developer for the role</p>
          <ButtonWrapper href="add-job.html" bg="bg-green-500"> Add Job </ButtonWrapper>
        </Card>
      </div>
    </div>
  </section>
</template>
