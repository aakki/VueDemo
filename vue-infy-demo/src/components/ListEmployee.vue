<template>
  <div class="ma-4">
    <v-row class="py-2">
      <v-col cols="12">
        <table-view
          :headers="headers"
          :employees="employees"
          :title="tableTitle"
        />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import TableView from '../components/shared/TableView';

export default {
  name: 'EmployeeList',
  components: {
    TableView
  },
  data() {
    return {
      tableTitle: 'List of Employees',
      headers: [
        {
          text: 'Name',
          align: 'start',
          sortable: false,
          value: 'name'
        },
        { text: 'Age', value: 'age' },
        { text: 'Email', value: 'email' },
        { text: 'Education', value: 'education' },
        { text: 'Actions', value: 'actions', sortable: false }
      ],
      employees: [
        {
          name: 'Albert',
          age: 30,
          email: 'albert@gmail.com',
          education: 'MCA'
        },
        {
          name: 'Daniel',
          age: 45,
          email: 'daniel@gmail.com',
          education: 'BCA'
        },
        {
          name: 'ADaniel',
          age: 35,
          email: 'adaniel@gmail.com',
          education: 'B.Tech'
        },
        {
          name: 'BDaniel',
          age: 36,
          email: 'bdaniel@gmail.com',
          education: 'M.Tech'
        },
        {
          name: 'cDaniel',
          age: 38,
          email: 'cdaniel@gmail.com',
          education: 'BE in Computer Science'
        },
        {
          name: 'dDaniel',
          age: 39,
          email: 'ddaniel@gmail.com',
          education: 'ME in Computer Science'
        }
      ]
    };
  }
};
</script>
