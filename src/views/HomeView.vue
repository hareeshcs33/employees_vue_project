<template>
  <div class="home">
    <EmployeesList
      :employees="employees"
      @addEmployee="addEmployee"
      @deleteEmployee="deleteEmployee"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import EmployeesList from "@/components/EmployeesList.vue";

export default {
  name: "HomeView",
  data() {
    return {
      employees: [
        {
          name: "Albert",
          age: 30,
          email: "albert@gmail.com",
          id: 1,
        },
        {
          name: "Daniel",
          age: 45,
          email: "daniel@gmail.com",
          id: 2,
        }
      ]
    };
  },
  methods: {
    addEmployee(employee) {
      this.employees.push(employee);
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter((emp) => emp.id !== id);
    }
  },
  components: {
    EmployeesList
  }
};
</script>
