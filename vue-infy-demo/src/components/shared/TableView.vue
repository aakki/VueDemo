<template>
  <div>
    <v-snackbar
      v-model="isAlert"
      color="success"
      :timeout="-1"
      :value="true"
      absolute
      outlined
    >
      {{ message }}
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="isAlert = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>

    <v-data-table
      :headers="headers"
      :items="allEmployees"
      :items-per-page="5"
      class="elevation-1"
      append-icon="mdi-magnify"
      :search="search"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>{{ title }}</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-row class="mt-4">
            <v-col cols="12">
              <v-text-field
                v-model="search"
                label="Search"
                class="mx-4"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-spacer></v-spacer>
          <v-dialog v-model="addEditDialog" max-width="600px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                Add Employee
              </v-btn>
            </template>

            <v-card>
              <v-card-title>
                <h3>{{ formTitle }}</h3>
              </v-card-title>
              <v-form
                ref="frmEmployee"
                class="d-flex flex-column align-start pa-4"
              >
                <v-card-text class="my-4">
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field
                          v-model="employeeModel.name"
                          label="Name"
                          outlined
                          :rules="rules"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field
                          v-model="employeeModel.age"
                          label="Age"
                          outlined
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field
                          v-model="employeeModel.email"
                          label="Email"
                          outlined
                          :rules="rules"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="6" md="6">
                        <v-text-field
                          v-model="employeeModel.education"
                          label="Education"
                          outlined
                          :rules="rules"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions class="ma-auto">
                  <v-btn
                    color="red"
                    depressed
                    @click="close"
                    class="text-capitalize mx-2"
                  >
                    Cancel
                  </v-btn>
                  <v-btn
                    color="green"
                    depressed
                    @click="onSave"
                    class="text-capitalize"
                  >
                    Save
                  </v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-dialog>
          <v-dialog v-model="deleteDialog" max-width="500px">
            <v-card color="pa-2">
              <v-card-title>
                <h4>Are you sure you want to delete this employee?</h4>
              </v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red" depressed @click="closeDelete">Cancel</v-btn>
                <v-btn color="green" depressed @click="deleteEmployeeConfirm">
                  OK
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item="{ item }">
        <tr>
          <td>
            <span>{{ item.name }}</span>
          </td>
          <td>
            <span>{{ item.age }}</span>
          </td>
          <td>
            <span>{{ item.email }}</span>
          </td>
          <td>
            <span>{{ item.education }}</span>
          </td>
          <td>
            <v-icon small class="mr-2" @click="editEmployee(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteEmployee(item)"> mdi-delete </v-icon>
          </td>
        </tr>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  name: 'TableView',
  props: {
    headers: { type: Array, default: () => [] },
    employees: { type: Array, default: () => [] },
    title: { type: String, default: () => '' }
  },
  data() {
    return {
      search: '',
      rules: [
        (value) => !!value || 'This field is required.',
        (value) => (value && value.length >= 3) || 'Minimum 3 characters'
      ],
      message: '',
      isAlert: false,
      addEditDialog: false,
      deleteDialog: false,
      allEmployees: [],
      editedIndex: -1,
      employeeModel: {
        name: '',
        age: 1,
        email: '',
        education: ''
      },
      defaultEmployee: {
        name: '',
        age: 1,
        email: '',
        education: ''
      }
    };
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add Employee' : 'Edit Employee';
    }
  },
  watch: {
    addEditDialog(val) {
      val || this.close();
    },
    deleteDialog(val) {
      val || this.closeDelete();
    }
  },
  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.allEmployees = this.employees;
    },
    editEmployee(o) {
      this.editedIndex = this.employees.indexOf(o);
      this.employeeModel = Object.assign({}, o);
      this.addEditDialog = true;
    },

    deleteEmployee(o) {
      this.editedIndex = this.employees.indexOf(o);
      this.employeeModel = Object.assign({}, o);
      this.deleteDialog = true;
    },

    deleteEmployeeConfirm() {
      this.allEmployees.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.addEditDialog = false;
      this.$refs.frmEmployee.reset();
      this.$nextTick(() => {
        this.employeeModel = Object.assign({}, this.defaultEmployee);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.deleteDialog = false;
      this.$nextTick(() => {
        this.employeeModel = Object.assign({}, this.defaultEmployee);
        this.editedIndex = -1;
      });
    },

    onSave(e) {
      this.$refs.frmEmployee.validate();
      console.log(
        this.$refs.frmEmployee.validate(),
        'this.$refs.frmEmployee.validate()'
      );
      if (
        !!this.employeeModel.name &&
        !!this.employeeModel.education &&
        !!this.employeeModel.email
      ) {
        if (this.editedIndex > -1) {
          Object.assign(
            this.allEmployees[this.editedIndex],
            this.employeeModel
          );
          this.message = 'Employee details has been updated successfully.';
        } else {
          this.allEmployees.push(this.employeeModel);
          this.message = 'Employee details has been added successfully.';
        }
        this.isAlert = true;
        this.close();
      } else {
        e.preventDefault();
        return;
      }
    }
  }
};
</script>
