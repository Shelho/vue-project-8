<template>
  <div class="container" >
    <form class="card" @submit.prevent="createPerson">
      <h2>Работа с базой данных</h2>
    
    <div class="form-control">
      <label for="name">введите имя</label>
      <input type="text" id="name" v-model.trim="name">
    </div>
    <button class="btn primary" :disabled="name.length === 0">создать человека</button>
  </form>
  </div>
</template>

<script> 
export default {
  data(){
    return{
      name:''
    }
  },
  methods:{
   async createPerson(){ 
       const response = await fetch('https://vue-with-htpp-c2499-default-rtdb.firebaseio.com/people.json',{
          method:'POST',
          headers:{
            'Content-Type':'application/json'
          }, 
          body:JSON.stringify({
            firsName:this.name
          })
        })
      const firebaseData = await response.json()
      console.log(firebaseData) 
      this.name = ""
    }
  }
}
</script>

<style>

</style>
