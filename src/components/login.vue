<template>
  <div>
    <div class="pa-8 blue-grey darken-2 white--text">
      <h1 class="text-center">Employee - Form</h1>
    </div>
    <v-container fill-height fluid>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="4">
          <v-card class="elevation-12">
            <v-toolbar color="primary" dark flat>
              <v-toolbar-title>Employee Form</v-toolbar-title>
            </v-toolbar>
            <v-card-text>
              <v-form>
                <v-text-field label="Employee Name" v-model="name" type="text" />
                <v-text-field label="Employee Salary" v-model="salary" type="number" />
                <v-text-field label="Employee Age" v-model="age" type="number" />
              </v-form>
            </v-card-text>
            <v-card-actions>
              <p class="red--text ml-3">{{errors}}</p>
              <v-spacer />
              <v-btn v-if="!edited" color="primary" @click="add()">Add</v-btn>
              <v-btn v-if="edited" color="primary" @click="add()">Update</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
        <div class="pa-3">
          <h2 class="pa-3">List Of Employee :</h2>
          <v-simple-table fixed-header height="285px" class="elevation-2">
            <thead>
              <tr>
                <th>Name</th>
                <th>Salary</th>
                <th>Age</th>
                <th>Edit</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(employee,index) in data" :key="index">
                <td>{{employee.employee_name}}</td>
                <td>{{employee.employee_salary}}</td>
                <td>{{employee.employee_age}}</td>
                <td>
                  <a @click="edit(employee)">Edit</a>
                </td>
                <td>
                  <a @click="del(employee)">Delete</a>
                </td>
              </tr>
            </tbody>
          </v-simple-table>
        </div>
      </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  name: "login",
  data: () => {
    return {
      status: "See Here",
      name: "",
      age: "",
      salary: "",
      data: [],
      editindex: "",
      edited: false,
      errors: ""
    };
  },
  mounted() {
    this.getEmployee();
  },
  methods: {
    add() {
      if (this.name == "") {
        this.errors = "* Name Required";
      } else if (this.salary == "") {
        this.errors = "* Salary Required";
      } else if (this.age == "") {
        this.errors = "* Age Required";
      } else {
        var payload = {
          name: this.name,
          salary: this.salary,
          age: this.age
        };
        if (this.edited == true) {
          this.axios
            .put(
              `http://dummy.restapiexample.com/api/v1/update/${this.editindex}`,
              payload
            )
            .then(response => {
              // eslint-disable-next-line
              console.log("edit", response);
            })
            .catch(error => {
              // eslint-disable-next-line
              console.log("error", error);
            });
          this.edited = false;
        } else {
          this.axios
            .post("http://dummy.restapiexample.com/api/v1/create", payload)
            .then(response => {
              if (response.status === 200) {
                // eslint-disable-next-line
                console.log(response, "added");
              }
            })
            .catch(function(error) {
              // eslint-disable-next-line
              console.log(error, "error");
              this.status = "error occured" + error;
            });
        }
        this.name = "";
        this.salary = "";
        this.age = "";
        this.errors = "";
      }
    },
    getEmployee() {
      this.axios
        .get("http://dummy.restapiexample.com/api/v1/employees")
        .then(response => {
          this.data = response.data;
        })
        .catch(function(error) {
          // eslint-disable-next-line
          console.log(error, "error");
        });
    },
    del(employee) {
      this.axios
        .delete(`http://dummy.restapiexample.com/api/v1/delete/${employee.id}`)
        .then(response => {
          // eslint-disable-next-line
          console.log("delete", response);
        })
        .catch(error => {
          // eslint-disable-next-line
          console.log("error", error);
        });
    },
    edit(employee) {
      this.edited = true;
      this.name = employee.employee_name;
      this.age = employee.employee_age;
      this.salary = employee.employee_salary;
      this.editindex = employee.id;
    }
  }
};
</script>
<style scoped>
</style>>