<template>
  <div class="min-h-screen flex flex-col bg-gray-50">
    <AppHeader />

    <main class="flex-1 max-w-5xl mx-auto w-full px-4 py-8">
      <StudentForm
        :editing-student="studentBeingEdited"
        @add-student="addStudent"
        @update-student="updateStudent"
        @cancel-edit="studentBeingEdited = null"
      />

      <StudentList
        :students="students"
        @edit-student="startEdit"
        @delete-student="deleteStudent"
      />
    </main>

    <AppFooter />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import AppHeader from './components/AppHeader.vue'
import StudentForm from './components/StudentForm.vue'
import StudentList from './components/StudentList.vue'
import AppFooter from './components/AppFooter.vue'

const students = ref([])
const studentBeingEdited = ref(null)
const STORAGE_KEY = 'module7-student-records'

// Step: Load records from localStorage
onMounted(() => {
  const saved = localStorage.getItem(STORAGE_KEY)
  students.value = saved ? JSON.parse(saved) : []
})

function saveStudents() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(students.value))
}

// Create
function addStudent(newStudent) {
  students.value.push({
    id: Date.now(),
    ...newStudent
  })
  saveStudents()
}

// Update
function updateStudent(updatedStudent) {
  const index = students.value.findIndex(s => s.id === updatedStudent.id)
  if (index !== -1) {
    students.value[index] = updatedStudent
    saveStudents()
  }
  studentBeingEdited.value = null
}

// Delete (with confirmation)
function deleteStudent(id) {
  const confirmed = window.confirm('Are you sure you want to delete this student record?')
  if (!confirmed) return
  students.value = students.value.filter(student => student.id !== id)
  saveStudents()
}

// Edit mode
function startEdit(student) {
  studentBeingEdited.value = { ...student }
}
</script>