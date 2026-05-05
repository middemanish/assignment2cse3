<template>
  <div class="app-wrapper">
    <div class="container py-5">
      <div class="text-center mb-5">
        <h1 class="app-title">Employee Management System</h1>
      </div>
      <div class="row g-4">
        <div class="col-lg-4">
          <EmployeeForm 
            @employee-saved="handleEmployeeSaved" 
            :employeeToEdit="selectedEmployee" 
            @clear-edit="selectedEmployee = null" 
          />
        </div>
        <div class="col-lg-8">
          <EmployeeList 
            :employees="employees" 
            :highlightedEmployeeId="highlightedEmployeeId"
            @edit-employee="setEmployeeToEdit" 
            @employee-deleted="fetchEmployees" 
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import EmployeeForm from './components/EmployeeForm.vue';
import EmployeeList from './components/EmployeeList.vue';

export default {
  name: 'App',
  components: {
    EmployeeForm,
    EmployeeList
  },
  data() {
    return {
      employees: [],
      selectedEmployee: null,
      highlightedEmployeeId: null
    };
  },
  methods: {
    fetchEmployees() {
      axios.get('https://69f9cc94c509a40d3aa356a9.mockapi.io/api/employees')
        .then(response => {
          this.employees = response.data;
        })
        .catch(error => {
          console.error("Error fetching employees:", error);
        });
    },
    setEmployeeToEdit(employee) {
      this.selectedEmployee = employee;
    },
    handleEmployeeSaved(employeeId) {
      this.fetchEmployees();
      if (employeeId) {
        this.highlightedEmployeeId = employeeId;
        setTimeout(() => {
          if (this.highlightedEmployeeId === employeeId) {
            this.highlightedEmployeeId = null;
          }
        }, 3000);
      }
    }
  },
  mounted() {
    this.fetchEmployees();
  }
}
</script>

<style>
body {
  font-family: 'Inter', sans-serif;
  background-color: #0d1117;
  color: #e6edf3;
  margin: 0;
  min-height: 100vh;
}

.app-wrapper {
  min-height: 100vh;
  background: radial-gradient(circle at top center, #161b22 0%, #0d1117 100%);
}

.app-title {
  font-weight: 700;
  letter-spacing: -0.05em;
  background: -webkit-linear-gradient(45deg, #79c0ff, #d2a8ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.glass-card {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.glass-input {
  background: rgba(0, 0, 0, 0.2) !important;
  border: 1px solid rgba(255, 255, 255, 0.1) !important;
  color: #e6edf3 !important;
  border-radius: 8px;
  transition: all 0.2s ease;
}

.glass-input:focus {
  border-color: #79c0ff !important;
  box-shadow: 0 0 0 0.25rem rgba(121, 192, 255, 0.2) !important;
  background: rgba(0, 0, 0, 0.4) !important;
}

.glass-input::placeholder {
  color: rgba(255, 255, 255, 0.3);
}
</style>
