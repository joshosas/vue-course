<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

const router = useRouter()

const form = ref({
  title: '',
  type: 'Full-Time',
  description: '',
  location: '',
  salary: '$70K - $80K',
  company: {
    name: '',
    description: '',
    contactEmail: '',
    contactPhone: '',
  },
})

// Error state tracking
const errors = ref({
  title: '',
  location: '',
  contactEmail: '',
  contactPhone: '',
  description: '',
})

// Validation Logic
const validationForm = () => {
  let isValid = true

  errors.value = { title: '', location: '', contactEmail: '', contactPhone: '', description: '' }

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

  if (!form.value.company.contactPhone.trim()) {
    errors.value.company.contactPhone = 'Phone Number is required.'
    isValid = false
  }

  // Basic email regex pattern check
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
  // Stop if validation fails
  if (!validationForm()) return

  // Continue if all form fields are valid
  try {
    const response = await axios.post('http://localhost:5000/jobs', form.value)
    router.push(`/jobs/${response.data.id}`)
  } catch (error) {
    console.error('Error adding job', error)
  }
}
</script>

<template>
  <section class="bg-green-50">
    <div class="container m-auto max-w-2xl py-24">
      <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
        <!-- novalidate prevents the browser from using native validation behaviors -->
        <form @submit.prevent="handleSubmit" novalidate>
          <h2 class="text-3xl text-center font-semibold mb-6">Add Job</h2>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Job Type</label>
            <select
              v-model="form.type"
              id="type"
              name="type"
              class="border rounded w-full py-2 px-3"
            >
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
              placeholder="eg. Senior Vue Developer"
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
              placeholder="Add any job duties, expectations, requirements, etc"
            ></textarea>
            <p v-if="errors.description" class="text-red-500 text-sm font-medium">
              {{ errors.description }}
            </p>
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Salary</label>
            <select
              v-model="form.salary"
              id="salary"
              name="salary"
              class="border rounded w-full py-2 px-3"
            >
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
              placeholder="Company Location"
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
              placeholder="Company Name"
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Company Description</label>
            <textarea
              v-model="form.company.description"
              class="border rounded w-full py-2 px-3"
              rows="4"
              placeholder="What does your company do?"
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
              placeholder="Email address for applicants"
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
              :class="[
                'border rounded w-full py-2 px-3',
                errors.contactPhone ? 'border-red-500' : '',
              ]"
              placeholder="Add phone number"
            />
            <p v-if="errors.contactPhone" class="text-red-500 text-sm font-medium">
              {{ errors.contactPhone }}
            </p>
          </div>

          <div>
            <button
              class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
              type="submit"
            >
              Add Job
            </button>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>
