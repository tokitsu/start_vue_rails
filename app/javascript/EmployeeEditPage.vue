<template>
  <employee-form-pane :errors="errors" :employee="employee" @submit="updateEmployee"></employee-form-pane>
</template>

<script>
import axios from 'axios';

import EmployeeFormPane from 'EmployeeFormPane.vue'

export default {
  components:{
    EmployeeFormPane
  },
  data: function () {
    return {
      employee: {},
      errors: ''
    }
  },
  mounted () {
    axios.get(`api/v1/employees/${this.$route.params.id}.json`)
    .then(response => (this.employee = response.data))
  },
  methods: {
    updateEmployee: function(){
      axios.patch(`api/v1/employees/${this.employee.id}.json`, this.employee)
      .then(response => {
        let e = response.data;
        this.$router.push({
          name: 'EmployeeDetailPage', params: { id: e.id } });
      })
      .catch(error => {
        console.error(error);
        if (error.response.data && error.response.data.errors){
          this.errors = error.response.data.errors;
        }
      })
    },
  }
}
</script>

