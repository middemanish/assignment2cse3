<template>
  <div class="glass-card overflow-hidden">
    <div class="px-4 py-3 border-bottom border-secondary d-flex justify-content-between align-items-center">
      <h5 class="mb-0 fw-semibold text-light">Employee Directory</h5>
    </div>
    <div class="card-body p-0">
      <div class="table-responsive">
        <table class="table table-borderless table-hover glass-table mb-0 align-middle">
          <thead>
            <tr>
              <th class="text-uppercase small text-muted fw-bold pb-3">ID</th>
              <th class="text-uppercase small text-muted fw-bold pb-3">Name</th>
              <th class="text-uppercase small text-muted fw-bold pb-3">Designation</th>
              <th class="text-uppercase small text-muted fw-bold pb-3">Department</th>
              <th class="text-uppercase small text-muted fw-bold pb-3">Salary</th>
              <th class="text-uppercase small text-muted fw-bold pb-3 text-end pe-4">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="employees.length === 0">
              <td colspan="6" class="text-center py-5 text-muted">No employees found.</td>
            </tr>
            <tr v-for="employee in employees" 
                :key="employee.id || employee.Employees || employee.ID"
                :class="{ 'highlight-active': (employee.id || employee.Employees || employee.ID) == highlightedEmployeeId }"
                class="employee-row">
              <td class="text-muted">#{{ employee.id || employee.Employees || employee.ID }}</td>
              <td class="fw-medium">{{ employee.name || employee.Name }}</td>
              <td><span class="badge bg-secondary bg-opacity-25 text-light fw-normal">{{ employee.designation || employee.Designation }}</span></td>
              <td>{{ employee.department || employee.Department }}</td>
              <td>${{ employee.salary || employee.Salary }}</td>
              <td class="text-end pe-4">
                <button class="btn btn-sm btn-outline-primary me-2 rounded-pill px-3" @click="editEmployee(employee)">
                  Edit
                </button>
                <button class="btn btn-sm btn-outline-danger rounded-pill px-3" @click="deleteEmployee(employee.id || employee.Employees || employee.ID)">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'EmployeeList',
  props: {
    employees: {
      type: Array,
      required: true
    },
    highlightedEmployeeId: {
      type: [String, Number],
      default: null
    }
  },
  methods: {
    editEmployee(employee) {
      this.$emit('edit-employee', employee);
    },
    deleteEmployee(id) {
      if (confirm('Are you sure you want to delete this employee?')) {
        axios.delete(`https://69f9cc94c509a40d3aa356a9.mockapi.io/api/employees/${id}`)
          .then(() => {
            this.$emit('employee-deleted');
          })
          .catch(error => console.error("Error deleting employee:", error));
      }
    }
  }
}
</script>

<style scoped>
.glass-table {
  --bs-table-bg: transparent;
  --bs-table-color: #e6edf3;
  --bs-table-hover-bg: rgba(255, 255, 255, 0.05);
}

.glass-table th {
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  letter-spacing: 0.05em;
}

.glass-table td {
  border-bottom: 1px solid rgba(255, 255, 255, 0.03);
}

.employee-row {
  transition: all 0.3s ease;
}

.highlight-active td {
  animation: highlightPulse 3s ease-out;
}

@keyframes highlightPulse {
  0% {
    background-color: rgba(121, 192, 255, 0.25);
  }
  10% {
    background-color: rgba(121, 192, 255, 0.35);
  }
  100% {
    background-color: transparent;
  }
}
</style>
