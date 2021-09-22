<template>
<div class="container">
  <Header title="taskTracker" />
  <Task @color-changer="changeColor" @tas-delete="deleteTask" :task="task"/>
  <h1> HelloWorld </h1>

</div>
 
</template>

<script>
import Header from './components/Header.vue'
import Task from './components/Task'

export default {
  name: 'App',
  components: {
    Header,
    Task,
  },
  data(){
    return{
    task: []
    }
  },
  created() {
    this.task = [
        {
          id: 1,
          text: 'Dr Milbourne',
          day: '16 kweitnia',
          reminder: true,
        },
          {
          id: 2,
          text: 'Dr otis',
          day: '32 maja',
          reminder: true,
        },
          {
          id: 3,
          text: 'Dr ciamajda',
          day: '299 lutego',
          reminder: false,
        },
          


    ]

  },
  methods:{
    deleteTask(id){
      if(confirm('u sure?')){
        this.task = this.task.filter((tas) => tas.id !== id)

      }

  },
    changeColor(id){
      this.task = this.task.map((tas) => tas.id === id ? {...tas, reminder: !tas.reminder} : tas)
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
