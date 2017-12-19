<template>
  <div id="dashboard">
      <ul class="collection with-header">
        <li class="collection-header"><h4 class="grey-text">Employees</h4></li>
        <li v-for="employee in employees" v-bind:key="employee.id" class="collection-item"
        ><div class="chip blue-text">{{employee.dept}}</div>{{employee.employee_id}}:{{employee.name}} 
        <router-link class="secondary-content" v-bind:to="{name: 'view-employee', params: {employee_id: employee.employee_id}}">
        <i class="material-icons blue-text">remove_red_eye</i>
        </router-link>
        </li>
      </ul>
            
      <div class="fixed-action-btn">
        <router-link to="/new" class="btn-floating waves-effect waves-light hoverable btn-large red">
          <i class="large material-icons">add</i>
        </router-link>
        <!-- <ul>
          <li><a class="btn-floating red"><i class="material-icons">insert_chart</i></a></li>
          <li><a class="btn-floating yellow darken-1"><i class="material-icons">format_quote</i></a></li>
          <li><a class="btn-floating green"><i class="material-icons">publish</i></a></li>
          <li><a class="btn-floating blue"><i class="material-icons">attach_file</i></a></li>
        </ul> -->
  </div>
  </div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'dashboard',
  data () {
      return {
        employees : []
      }
  },
  created (){
    db.collection('employees').orderBy('dept').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        const data = {
          'id': doc.id,
          'employee_id': doc.data().employee_id,
          'name': doc.data().name,
          'dept': doc.data().dept,
          'position': doc.data().position
        }
        this.employees.push(data)
      })
    })
  }
}
</script>
