<template>
  <div class="bg-white rounded-lg shadow p-6">
    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3 mb-4">
      <h2 class="text-lg font-semibold text-gray-800">
        Student Records
        <span class="text-sm font-normal text-gray-500">({{ students.length }} total)</span>
      </h2>

      <input v-model="searchTerm" type="text" placeholder="Search by last name..."
        class="border border-gray-300 rounded-md px-3 py-2 w-full sm:w-64 focus:outline-none focus:ring-2 focus:ring-emerald-500" />
    </div>

    <div v-if="filteredStudents.length === 0" class="text-center text-gray-500 py-8">
      No students found.
    </div>

    <div v-else class="overflow-x-auto">
      <table class="w-full text-sm text-left">
        <thead class="bg-gray-100 text-gray-600 uppercase text-xs">
          <tr>
            <th class="px-4 py-2">Student ID</th>
            <th class="px-4 py-2">Name</th>
            <th class="px-4 py-2">Course</th>
            <th class="px-4 py-2">Year Level</th>
            <th class="px-4 py-2">Email</th>
            <th class="px-4 py-2 text-right">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="student in filteredStudents" :key="student.id" class="border-b hover:bg-gray-50">
            <td class="px-4 py-2 font-medium text-gray-800">{{ student.studentId }}</td>
            <td class="px-4 py-2">{{ student.firstName }} {{ student.lastName }}</td>
            <td class="px-4 py-2">{{ student.course }}</td>
            <td class="px-4 py-2">{{ student.yearLevel }}</td>
            <td class="px-4 py-2">{{ student.email }}</td>
            <td class="px-4 py-2 text-right space-x-2">
              <button @click="$emit('edit-student', student)"
                class="text-emerald-600 hover:underline text-sm">Edit</button>
              <button @click="$emit('delete-student', student.id)"
                class="text-red-600 hover:underline text-sm">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  students: { type: Array, required: true }
})
defineEmits(['edit-student', 'delete-student'])

const searchTerm = ref('')

const filteredStudents = computed(() => {
  const keyword = searchTerm.value.toLowerCase().trim()
  if (!keyword) return props.students
  return props.students.filter(student => student.lastName.toLowerCase().includes(keyword))
})
</script>