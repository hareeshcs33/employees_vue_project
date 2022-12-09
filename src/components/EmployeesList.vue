<template>
  <div class="employees-list">
    <h3 class="text-center">Add Employees</h3>
    <div class="form-block">
      <div class="form-control">
        <label for="name">Name</label>
        <input type="text" v-model="name" placeholder="Enter Name" />
      </div>
      <div class="form-control">
        <label for="age">Age</label>
        <input type="number" v-model="age" placeholder="Enter Age" />
      </div>
      <div class="form-control">
        <label for="email">Email</label>
        <input type="email" v-model="email" placeholder="Enter Email" />
      </div>
      <button
        class="add-btn"
        @click="AddEmployee"
        :disabled="!this.validateEmployeeData"
      >
        Add Employee
      </button>
    </div>

    <h3 class="text-center">Employees Table</h3>
    <input
      type="text"
      class="search-box"
      v-model="searchEmployee"
      placeholder="Search Employee Name ..."
    />
    <table class="employees-table" v-if="this.filteredEmployeeList.length">
      <thead>
        <tr>
          <th>No.</th>
          <th>Name</th>
          <th>Age</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(employee, index) in this.filteredEmployeeList"
          :key="employee.id"
          @click="selectEmployee(employee.id)"
          :class="{ highlight: employee.id == this.selectedEmployeeId }"
        >
          <td>{{ index }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.age }}</td>
          <td>{{ employee.email }}</td>
          <td>
            <button
              class="delete"
              @click="deleteEmployee(employee.id)"
              :disabled="employee.id !== this.selectedEmployeeId"
            >
              &times;
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "EmployeesList",
  props: {
    employees: {
      type: Array
    }
  },
  data() {
    return {
      searchEmployee: "",
      name: "",
      age: "",
      email: "",
      selectedEmployeeId: null
    }
  },
  computed: {
    filteredEmployeeList() {
      return this.employees?.filter(
        (emp) => !emp.name.toLowerCase().indexOf(this.searchEmployee)
      );
    },
    validateEmployeeData() {
      return this.name == "" ||
        this.age == "" ||
        this.email == "" ||
        !/^[^@]+@\w+(\.\w+)+\w$/.test(this.email)
        ? false
        : true;
    }
  },
  methods: {
    resetEmployee() {
      this.name = "";
      this.age = "";
      this.email = "";
    },
    AddEmployee() {
      if (!this.validateEmployeeData) {
        alert("Complete All Employee Fields");
      } else {
        this.$emit("addEmployee", {
          name: this.name,
          age: this.age,
          email: this.email,
          id: Date.now()
        });
        this.resetEmployee();
      }
    },
    selectEmployee(id) {
      this.selectedEmployeeId = id;
    },
    deleteEmployee(id) {
      this.$emit("deleteEmployee", id);
    }
  }
};
</script>

<style scoped>
@import "../styles/employee-list.css";
</style>
