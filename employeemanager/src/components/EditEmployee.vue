<template>
  <div id="edit-employee">
      <H3>Edit Employee</H3>
      <div class="row">
    <form @submit.prevent="updateEmployee" class="col s12">
      <div class="row">
        <div class="input-field col s12">
          <input disabled v-model="employee_id" type="text" class="validate" required>
          
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input v-model="name" type="text" class="validate" required>
          
        </div>
      </div>
      
      <div class="row">
        <div class="input-field col s12">
          <input v-model="position" type="text" class="validate" required>
          
        </div>
      </div>
      <div class="row">
        <div class="input-field col s12">
          <input v-model="dept" type="text" class="validate" required>
          
        </div>
      </div>
      <button type="submit" class="btn">Submit</button>
      <router-link v-bind:to="{name: 'view-employee', params: {employee_id: employee_id}}" class="btn grey">Cancel</router-link>
    </form>
  </div>
  </div>
</template>

<<script>
import db from './firebaseInit'
export default {
  name: 'edit-employee',
  data () {
      return {
          employee_id : null,
          name : null,
          dept : null,
          position : null
      }
  },
  beforeRouteEnter (to, from, next){
    db.collection('employees').where('employee_id', '==', to.params.employee_id).get().then(querySnapshot =>{
      querySnapshot.forEach(doc => {
        next(vm =>{
            vm.employee_id = doc.data().employee_id,
            vm.name = doc.data().name,
            vm.dept = doc.data().dept,
            vm.position = doc.data().position
        })
      })
    })
  },
  watch: {
    '$route' : 'fetchData'
  },
  methods : {
    fetchData () {
      db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get()
      .then(querySnapshot =>{
          querySnapshot.forEach(doc => {
              this.employee_id = doc.data().employee_id,
              this.name = doc.data().name,
              this.dept = doc.data().dept,
              this.position = doc.data().position
      })
    })
    },
    updateEmployee () {
      db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get()
      .then(querySnapshot =>{
          querySnapshot.forEach(doc => {
              doc.ref.update({
                employee_id: this.employee_id,
                name: this.name,
                dept: this.dept,
                position: this.position
              }).then(() => {
                this.$router.push({name: 'view-employee', params: {employee_id: this.employee_id}})
              })
      })
    })
    }
  }
}
</script>
