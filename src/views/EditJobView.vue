<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import axios from 'axios'

const route = useRoute()
const router = useRouter()
const jobId = route.params.id

const isLoading = ref(true)

const form = ref({
  title: '',
  type: '',
  description: '',
  location: '',
  salary: '',
  company: {
    name: '',
    description: '',
    contactEmail: '',
    contactPhone: '',
  },
})

const errors = ref({
  title: '',
  location: '',
  contactEmail: '',
  description: '',
})

// Fetch current values to populate form
onMounted(async () => {
  try {
    const response = await axios.get(`http://localhost:5000/jobs/${jobId}`)
    form.value = response.data
  } catch (error) {
    console.error('Error fetching job details for edit:', error)
  } finally {
    isLoading.value = false
  }
})

const validateForm = () => {
  let isValid = true
  errors.value = { title: '', location: '', contactEmail: '', description: '' }

  if (!form.value.title.trim()) {
    errors.value.title = 'Job listing name is required.'
    isValid = false
  }
  if (!form.value.description.trim()) {
    errors.value.description = 'Please provide a job description.'
    isValid = false
  }
  if (!form.value.location.trim()) {
    errors.value.location = 'Job location is required.'
    isValid = false
  }
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!form.value.company.contactEmail.trim()) {
    errors.value.contactEmail = 'Contact email is required.'
    isValid = false
  } else if (!emailPattern.test(form.value.company.contactEmail)) {
    errors.value.contactEmail = 'Please enter a valid email address.'
    isValid = false
  }

  return isValid
}

const handleSubmit = async () => {
  if (!validateForm()) return

  try {
    // Send updated object back using a PUT request
    await axios.put(`http://localhost:5000/jobs/${jobId}`, form.value)
    router.push(`/jobs/${jobId}`) // Send back to details view
  } catch (error) {
    console.error('Error updating job:', error)
  }
}
</script>

<template>
  <div v-if="isLoading" class="text-center py-24 text-gray-500">
    Loading job data for editing...
  </div>

  <section v-else class="bg-green-50">
    <div class="container m-auto max-w-2xl py-24">
      <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
        <form @submit.prevent="handleSubmit" novalidate>
          <h2 class="text-3xl text-center font-semibold mb-6">Update Job</h2>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Job Type</label>
            <select v-model="form.type" class="border rounded w-full py-2 px-3">
              <option value="Full-Time">Full-Time</option>
              <option value="Part-Time">Part-Time</option>
              <option value="Remote">Remote</option>
              <option value="Internship">Internship</option>
            </select>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Job Listing Name</label>
            <input
              v-model="form.title"
              type="text"
              :class="[
                'border rounded w-full py-2 px-3 mb-1',
                errors.title ? 'border-red-500' : '',
              ]"
            />
            <p v-if="errors.title" class="text-red-500 text-sm font-medium">{{ errors.title }}</p>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Description</label>
            <textarea
              v-model="form.description"
              :class="[
                'border rounded w-full py-2 px-3 mb-1',
                errors.description ? 'border-red-500' : '',
              ]"
              rows="4"
            ></textarea>
            <p v-if="errors.description" class="text-red-500 text-sm font-medium">
              {{ errors.description }}
            </p>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Salary</label>
            <select v-model="form.salary" class="border rounded w-full py-2 px-3">
              <option value="Under $50K">Under $50K</option>
              <option value="$50K - $60K">$50K - $60K</option>
              <option value="$60K - $70K">$60K - $70K</option>
              <option value="$70K - $80K">$70K - $80K</option>
              <option value="$80K - $90K">$80K - $90K</option>
              <option value="$90K - $100K">$90K - $100K</option>
              <option value="Over $100K">Over $100K</option>
            </select>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Location</label>
            <input
              v-model="form.location"
              type="text"
              :class="[
                'border rounded w-full py-2 px-3 mb-1',
                errors.location ? 'border-red-500' : '',
              ]"
            />
            <p v-if="errors.location" class="text-red-500 text-sm font-medium">
              {{ errors.location }}
            </p>
          </div>

          <h3 class="text-2xl mb-5">Company Info</h3>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Company Name</label>
            <input
              v-model="form.company.name"
              type="text"
              class="border rounded w-full py-2 px-3"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Company Description</label>
            <textarea
              v-model="form.company.description"
              class="border rounded w-full py-2 px-3"
              rows="4"
            ></textarea>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Contact Email</label>
            <input
              v-model="form.company.contactEmail"
              type="email"
              :class="[
                'border rounded w-full py-2 px-3 mb-1',
                errors.contactEmail ? 'border-red-500' : '',
              ]"
            />
            <p v-if="errors.contactEmail" class="text-red-500 text-sm font-medium">
              {{ errors.contactEmail }}
            </p>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Contact Phone</label>
            <input
              v-model="form.company.contactPhone"
              type="tel"
              class="border rounded w-full py-2 px-3"
            />
          </div>

          <div>
            <button
              class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
              type="submit"
            >
              Update Job
            </button>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>
