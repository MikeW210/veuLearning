<template>
<div class="container">
  <Header @toggle-add-task="toggleAddTas" title="taskTracker" :showAddTas="showAddTas" />
  <div v-if="showAddTas">
  <Addtas @add-tas="addTas" />
  </div>
  <Task @color-changer="changeColor" @tas-delete="deleteTask" :task="task"/>
  <h1> HelloWorld </h1>

</div>
 
</template>

<script>
import Header from './components/Header.vue'
import Task from './components/Task'
import Addtas from './components/Addtas'

export default {
  name: 'App',
  components: {
    Header,
    Task,
    Addtas,
  },
  data(){
    return{
    task: [],
    showAddTas: false,
    }

  },
  async created() {
    this.task = await this.fetchTask()
        

  },
  methods:{
    async fetchTask(){
      const res = await fetch('api/task')
      const data = await res.json()
      return data
      
    }, 
    async fetchTas(id){
      const res = await fetch(`api/task/${id}`)
      const data = await res.json()
      return data
      
    },
    toggleAddTas(){
      this.showAddTas = !this.showAddTas
    },
    async addTas(newTas){
      const res = await fetch('api/task', {
        method: 'POST',
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(newTas),
      })

      this.task = [...this.task, newTas]

    },
    async deleteTask(id){
      if(confirm('u sure?')){
        const res = await fetch(`api/task/${id}`, {
        method: 'DELETE',
        })
        res.status === 200 ? (this.task = this.task.filter((tas) => tas.id !== id)) : alert('Errror deleting task')
        

      }

  },
    async changeColor(id){
      const tasToToggle = await this.fetchTas(id)
      const updateTas = {...tasToToggle, reminder: !tasToToggle.reminder}
      const res = await fetch(`api/task/${id}`, {
        method: 'PUT',
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(updateTas),
      })
      const data = await res.json()
      this.task = this.task.map((tas) => tas.id === id ? {...tas, reminder: !data.reminder} : tas)
    }

  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
</style>
