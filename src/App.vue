<script setup>
import { ref } from 'vue'

const students = ref([])

const showForm = ref(false)

const editingStudent = ref(null)

const student = ref({
  studentId: '',
  name: '',
  course: '',
  year: '',
  email: ''
})


// ====================
// ADD BUTTON
// ====================

function openAddForm() {
  editingStudent.value = null

  student.value = {
    studentId: '',
    name: '',
    course: '',
    year: '',
    email: ''
  }

  showForm.value = true
}


// ====================
// SAVE STUDENT
// ====================

function saveStudent() {

  if (
    !student.value.studentId ||
    !student.value.name ||
    !student.value.course ||
    !student.value.year ||
    !student.value.email
  ) {
    alert('Please fill in all fields.')
    return
  }

  // ADD
  if (editingStudent.value === null) {

    students.value.push({
      id: Date.now(),
      ...student.value
    })

    alert('Student added!')

  }

  // EDIT / UPDATE
  else {

    const index = students.value.findIndex(
      s => s.id === editingStudent.value
    )

    students.value[index] = {
      id: editingStudent.value,
      ...student.value
    }

    alert('Student updated!')
  }

  showForm.value = false
}


// ====================
// EDIT BUTTON
// ====================

function editStudent(s) {

  editingStudent.value = s.id

  student.value = {
    studentId: s.studentId,
    name: s.name,
    course: s.course,
    year: s.year,
    email: s.email
  }

  showForm.value = true
}


// ====================
// DELETE BUTTON
// ====================

function deleteStudent(id) {

  if (confirm('Delete this student?')) {

    students.value = students.value.filter(
      s => s.id !== id
    )

    alert('Student deleted!')
  }
}


// ====================
// CANCEL BUTTON
// ====================

function cancelForm() {
  showForm.value = false
}
</script>


<template>

  <div class="container">

    <h1>Student Management System</h1>


    <!-- ===================== -->
    <!-- ADD BUTTON -->
    <!-- ===================== -->

    <button
      class="add-button"
      @click="openAddForm"
    >
      + Add Student
    </button>


    <!-- ===================== -->
    <!-- ADD / EDIT FORM -->
    <!-- ===================== -->

    <div
      v-if="showForm"
      class="form-box"
    >

      <h2>
        {{
          editingStudent === null
            ? 'Add Student'
            : 'Edit Student'
        }}
      </h2>

      <input
        v-model="student.studentId"
        placeholder="Student ID"
      >

      <input
        v-model="student.name"
        placeholder="Student Name"
      >

      <input
        v-model="student.course"
        placeholder="Course"
      >

      <input
        v-model="student.year"
        placeholder="Year Level"
      >

      <input
        v-model="student.email"
        placeholder="Email"
      >


      <div class="form-buttons">

        <button
          class="save-button"
          @click="saveStudent"
        >
          {{
            editingStudent === null
              ? 'Add Student'
              : 'Update Student'
          }}
        </button>

        <button
          class="cancel-button"
          @click="cancelForm"
        >
          Cancel
        </button>

      </div>

    </div>


    <!-- ===================== -->
    <!-- STUDENT TABLE -->
    <!-- ===================== -->

    <div class="table-box">

      <h2>Students</h2>

      <table>

        <thead>

          <tr>
            <th>ID</th>
            <th>Student ID</th>
            <th>Name</th>
            <th>Course</th>
            <th>Year</th>
            <th>Email</th>
            <th>Actions</th>
          </tr>

        </thead>


        <tbody>

          <tr
            v-for="s in students"
            :key="s.id"
          >

            <td>
              {{ s.id }}
            </td>

            <td>
              {{ s.studentId }}
            </td>

            <td>
              {{ s.name }}
            </td>

            <td>
              {{ s.course }}
            </td>

            <td>
              {{ s.year }}
            </td>

            <td>
              {{ s.email }}
            </td>


            <!-- ===================== -->
            <!-- EDIT & DELETE BUTTONS -->
            <!-- ===================== -->

            <td class="actions">

              <button
                class="edit-button"
                @click="editStudent(s)"
              >
                Edit
              </button>

              <button
                class="delete-button"
                @click="deleteStudent(s.id)"
              >
                Delete
              </button>

            </td>

          </tr>


          <tr v-if="students.length === 0">

            <td
              colspan="7"
              class="empty"
            >
              No students yet.
            </td>

          </tr>

        </tbody>

      </table>

    </div>

  </div>

</template>