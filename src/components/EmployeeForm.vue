<template>
  <div class="glass-card mb-4 p-3">
    <div class="border-bottom border-secondary mb-4 pb-2">
      <h5 class="mb-0 fw-semibold text-light">{{ isEditing ? 'Edit Employee' : 'Add Employee' }}</h5>
    </div>
    <div class="card-body p-0">
      <form @submit.prevent="saveEmployee">
        <div class="mb-3">
          <label class="form-label text-light opacity-75 small text-uppercase fw-bold">Name</label>
          <input type="text" class="form-control glass-input" v-model="employee.name" required />
        </div>
        <div class="mb-3">
          <label class="form-label text-light opacity-75 small text-uppercase fw-bold">Designation</label>
          <input type="text" class="form-control glass-input" v-model="employee.designation" required />
        </div>
        <div class="mb-3">
          <label class="form-label text-light opacity-75 small text-uppercase fw-bold">Department</label>
          <input type="text" class="form-control glass-input" v-model="employee.department" required />
        </div>
        <div class="mb-4">
          <label class="form-label text-light opacity-75 small text-uppercase fw-bold">Salary</label>
          <input type="number" class="form-control glass-input" v-model="employee.salary" required />
        </div>
        <div class="d-grid gap-2">
          <button type="submit" class="btn" :class="isEditing ? 'btn-primary' : 'btn-success'" style="border-radius: 8px;">
            {{ isEditing ? 'Update Employee' : 'Add Employee' }}
          </button>
          <button type="button" class="btn btn-outline-light" v-if="isEditing" @click="cancelEdit" style="border-radius: 8px;">
            Cancel
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'EmployeeForm',
  props: {
    employeeToEdit: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      employee: {
        name: '',
        designation: '',
        department: '',
        salary: ''
      },
      isEditing: false
    };
  },
  watch: {
    employeeToEdit(newVal) {
      if (newVal) {
        this.employee = {
          id: newVal.id || newVal.Employees,
          name: newVal.name || newVal.Name || '',
          designation: newVal.designation || newVal.Designation || '',
          department: newVal.department || newVal.Department || '',
          salary: newVal.salary || newVal.Salary || ''
        };
        this.isEditing = true;
      } else {
        this.resetForm();
      }
    }
  },
  methods: {
    saveEmployee() {
      if (this.isEditing) {
        axios.put(`https://69f9cc94c509a40d3aa356a9.mockapi.io/api/employees/${this.employee.id}`, this.employee)
          .then(() => {
            this.$emit('employee-saved', this.employee.id);
            this.cancelEdit();
          })
          .catch(error => console.error("Error updating employee:", error));
      } else {
        axios.post('https://69f9cc94c509a40d3aa356a9.mockapi.io/api/employees', this.employee)
          .then((response) => {
            const newId = response.data.id || response.data.Employees || response.data.ID;
            this.$emit('employee-saved', newId);
            this.resetForm();
          })
          .catch(error => console.error("Error adding employee:", error));
      }
    },
    resetForm() {
      this.employee = {
        name: '',
        designation: '',
        department: '',
        salary: ''
      };
      this.isEditing = false;
    },
    cancelEdit() {
      this.resetForm();
      this.$emit('clear-edit');
    }
  }
}
</script>
