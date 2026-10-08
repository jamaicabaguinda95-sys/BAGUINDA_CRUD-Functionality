<script setup>
import { ref } from 'vue'

// =========================
// STUDENT DATA
// =========================

const students = ref(
  JSON.parse(localStorage.getItem('students')) || []
)

// =========================
// FORM DATA
// =========================

const studentId = ref('')
const name = ref('')
const course = ref('')
const yearLevel = ref('')
const email = ref('')

// Used to determine if we are editing
const editingId = ref(null)


// =========================
// SAVE TO LOCAL STORAGE
// =========================

function saveToStorage() {
  localStorage.setItem(
    'students',
    JSON.stringify(students.value)
  )
}


// =========================
// CREATE - ADD STUDENT
// =========================

function addStudent() {

  // Check if fields are empty
  if (
    !studentId.value ||
    !name.value ||
    !course.value ||
    !yearLevel.value ||
    !email.value
  ) {
    alert('Please fill in all fields.')
    return
  }

  const newStudent = {
    id: Date.now(),
    studentId: studentId.value,
    name: name.value,
    course: course.value,
    yearLevel: yearLevel.value,
    email: email.value
  }

  // Add student to array
  students.value.push(newStudent)

  // Save data
  saveToStorage()

  // Clear form
  clearForm()

  alert('Student added successfully!')
}


// =========================
// UPDATE - EDIT STUDENT
// =========================

function editStudent(student) {

  // Store the ID of the student being edited
  editingId.value = student.id

  // Put existing information into the form
  studentId.value = student.studentId
  name.value = student.name
  course.value = student.course
  yearLevel.value = student.yearLevel
  email.value = student.email
}


// =========================
// UPDATE - SAVE CHANGES
// =========================

function updateStudent() {

  const student = students.value.find(
    student => student.id === editingId.value
  )

  if (!student) {
    return
  }

  // Update student information
  student.studentId = studentId.value
  student.name = name.value
  student.course = course.value
  student.yearLevel = yearLevel.value
  student.email = email.value

  // Save changes
  saveToStorage()

  // Clear form
  clearForm()

  alert('Student updated successfully!')
}


// =========================
// DELETE - DELETE STUDENT
// =========================

function deleteStudent(id) {

  const confirmDelete = confirm(
    'Are you sure you want to delete this student?'
  )

  if (!confirmDelete) {
    return
  }

  // Remove student from array
  students.value = students.value.filter(
    student => student.id !== id
  )

  // Save changes
  saveToStorage()

  alert('Student deleted successfully!')
}


// =========================
// CLEAR FORM
// =========================

function clearForm() {

  studentId.value = ''
  name.value = ''
  course.value = ''
  yearLevel.value = ''
  email.value = ''

  editingId.value = null
}


// =========================
// SUBMIT FORM
// =========================

function submitForm() {

  if (editingId.value === null) {

    // CREATE
    addStudent()

  } else {

    // UPDATE
    updateStudent()

  }
}
</script>


<template>

  <div class="container">

    <h1>Student Management System</h1>


    <!-- ========================= -->
    <!-- ADD / EDIT FORM -->
    <!-- ========================= -->

    <div class="form-card">

      <h2>
        {{ editingId === null
          ? 'Add Student'
          : 'Edit Student'
        }}
      </h2>

      <form @submit.prevent="submitForm">

        <div class="form-group">

          <label>Student ID</label>

          <input
            v-model="studentId"
            type="text"
            placeholder="Example: 2026-001"
          >

        </div>


        <div class="form-group">

          <label>Name</label>

          <input
            v-model="name"
            type="text"
            placeholder="Enter student name"
          >

        </div>


        <div class="form-group">

          <label>Course</label>

          <input
            v-model="course"
            type="text"
            placeholder="Example: BSIT"
          >

        </div>


        <div class="form-group">

          <label>Year Level</label>

          <select v-model="yearLevel">

            <option value="">
              Select Year Level
            </option>

            <option value="1st Year">
              1st Year
            </option>

            <option value="2nd Year">
              2nd Year
            </option>

            <option value="3rd Year">
              3rd Year
            </option>

            <option value="4th Year">
              4th Year
            </option>

          </select>

        </div>


        <div class="form-group">

          <label>Email</label>

          <input
            v-model="email"
            type="email"
            placeholder="student@email.com"
          >

        </div>


        <!-- BUTTONS -->

        <div class="form-buttons">

          <button
            type="submit"
            class="btn btn-primary"
          >

            {{ editingId === null
              ? 'Add Student'
              : 'Update Student'
            }}

          </button>


          <button
            v-if="editingId !== null"
            type="button"
            class="btn btn-secondary"
            @click="clearForm"
          >
            Cancel
          </button>

        </div>

      </form>

    </div>


    <!-- ========================= -->
    <!-- STUDENT TABLE -->
    <!-- ========================= -->

    <div class="table-card">

      <h2>Student List</h2>

      <table>

        <thead>

          <tr>

            <th>#</th>
            <th>Student ID</th>
            <th>Name</th>
            <th>Course</th>
            <th>Year</th>
            <th>Email</th>
            <th>Actions</th>

          </tr>

        </thead>


        <tbody>

          <!-- Display students -->

          <tr
            v-for="(student, index) in students"
            :key="student.id"
          >

            <td>
              {{ index + 1 }}
            </td>

            <td>
              {{ student.studentId }}
            </td>

            <td>
              {{ student.name }}
            </td>

            <td>
              {{ student.course }}
            </td>

            <td>
              {{ student.yearLevel }}
            </td>

            <td>
              {{ student.email }}
            </td>


            <!-- ACTION BUTTONS -->

            <td class="actions">

              <!-- EDIT -->

              <button
                class="btn btn-edit"
                @click="editStudent(student)"
              >
                Edit
              </button>


              <!-- DELETE -->

              <button
                class="btn btn-delete"
                @click="deleteStudent(student.id)"
              >
                Delete
              </button>

            </td>

          </tr>


          <!-- No students -->

          <tr v-if="students.length === 0">

            <td
              colspan="7"
              class="no-data"
            >
              No students found.
            </td>

          </tr>

        </tbody>

      </table>

    </div>

  </div>

</template>