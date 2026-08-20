<template>
  <div class="bg-white rounded-lg shadow p-6 mb-6">
    <h2 class="text-lg font-semibold text-gray-800 mb-4">
      {{ editingId ? "Edit Student" : "Add a New Student" }}
    </h2>

    <form @submit.prevent="handleSubmit" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Student ID</label>
        <input v-model="form.studentId" type="text" placeholder="e.g. 2023-00123"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
        <input v-model="form.email" type="email"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">First Name</label>
        <input v-model="form.firstName" type="text"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Last Name</label>
        <input v-model="form.lastName" type="text"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Course</label>
        <input v-model="form.course" type="text" placeholder="e.g. BS Computer Science"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Year Level</label>
        <select v-model="form.yearLevel"
          class="w-full border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500">
          <option value="1">1st Year</option>
          <option value="2">2nd Year</option>
          <option value="3">3rd Year</option>
          <option value="4">4th Year</option>
        </select>
      </div>

      <p v-if="errorMessage" class="sm:col-span-2 text-sm text-red-600 bg-red-50 border border-red-200 rounded-md px-3 py-2">
        {{ errorMessage }}
      </p>

      <p v-if="successMessage" class="sm:col-span-2 text-sm text-green-700 bg-green-50 border border-green-200 rounded-md px-3 py-2">
        {{ successMessage }}
      </p>

      <div class="sm:col-span-2 flex gap-3">
        <button type="submit"
          class="bg-emerald-700 text-white px-4 py-2 rounded-md hover:bg-emerald-800 transition">
          {{ editingId ? "Update Student" : "Add Student" }}
        </button>
        <button v-if="editingId" type="button" @click="cancelEdit"
          class="bg-gray-200 text-gray-700 px-4 py-2 rounded-md hover:bg-gray-300 transition">
          Cancel
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  editingStudent: { type: Object, default: null }
})
const emit = defineEmits(['add-student', 'update-student', 'cancel-edit'])

const emptyForm = { studentId: '', firstName: '', lastName: '', course: '', yearLevel: '1', email: '' }
const form = ref({ ...emptyForm })
const editingId = ref(null)
const errorMessage = ref('')
const successMessage = ref('')

watch(() => props.editingStudent, (student) => {
  if (student) {
    form.value = { ...student }
    editingId.value = student.id
  }
})

function handleSubmit() {
  errorMessage.value = ''
  successMessage.value = ''

  if (!form.value.studentId.trim() || !form.value.firstName.trim() ||
      !form.value.lastName.trim() || !form.value.course.trim()) {
    errorMessage.value = 'Please fill in Student ID, First Name, Last Name, and Course before submitting.'
    return
  }

  if (editingId.value) {
    emit('update-student', { ...form.value, id: editingId.value })
    successMessage.value = 'Student updated successfully.'
  } else {
    emit('add-student', { ...form.value })
    successMessage.value = 'Student added successfully.'
  }

  form.value = { ...emptyForm }
  editingId.value = null

  setTimeout(() => { successMessage.value = '' }, 2500)
}

function cancelEdit() {
  form.value = { ...emptyForm }
  editingId.value = null
  emit('cancel-edit')
}
</script>