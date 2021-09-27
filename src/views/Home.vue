<template>
    
  <Addtas v-show="showAddTas" @add-tas="addTas" />
  
  <Task @color-changer="changeColor" @tas-delete="deleteTask" :task="task"/>
</template>

<script>
import Task from '../components/Task'
import Addtas from '../components/Addtas'
export default {
    name: 'Home',
    components: {
        Task,
        Addtas
    },
    data() {
        return{
            task: [],
        }
    },
    async created() {
    this.task = await this.fetchTask()
    },
    props: {
        showAddTas: Boolean,
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