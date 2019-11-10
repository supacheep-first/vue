<template>
  <div class="home small-container">
    <h1>Employee Table</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import EmployeeTable from "@/components/EmployeeTable.vue";
import EmployeeForm from "@/components/EmployeeForm.vue";
import axios from "axios";

export default {
  name: "home",
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    };
  },
  methods: {
    async addEmployee(employee) {
      try {
        const response = await axios
          .post("https://jsonplaceholder.typicode.com/users", {
            body: JSON.stringify(employee)
          })
          .then(res => {
            return JSON.parse(res.data.body);
          });
        this.employees = [...this.employees, response];
      } catch (error) {
        console.error(error);
      }
    },
    async deleteEmployee(id) {
      try {
        await axios.delete("https://jsonplaceholder.typicode.com/users/${id}");
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.log(error);
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await axios
          .put("https://jsonplaceholder.typicode.com/users/" + id, {
            body: JSON.stringify(updatedEmployee)
          })
          .then(res => {
            return JSON.parse(res.data.body);
          });
        this.employees = this.employees.map(employee =>
          employee.id === id ? response : employee
        );
      } catch (error) {
        console.error(error);
      }
    },
    async getEmployee() {
      try {
        this.employees = await axios(
          "https://jsonplaceholder.typicode.com/users"
        ).then(response => {
          return response.data;
        });
      } catch (error) {
        console.log(error);
      }
    }
  },
  mounted() {
    this.getEmployee();
  }
};
</script>
<style>
.small-container {
  max-width: 680px;
}
</style>
