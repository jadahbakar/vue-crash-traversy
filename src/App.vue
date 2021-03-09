<template>
<div class="container">

  <!-- <h1>Hello World</h1> -->
  <TheHeader @toggle-add-task="toggleAddTask" title="Task Tracker" :showIt="showAddTask" /> 
  <div v-show="showAddTask">
    <TaskAdder @add-task="addTask" /> 
  </div>
  <TaskList  @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TheHeader from './components/TheHeader'
import TaskList from './components/TaskList'
import TaskAdder from './components/TaskAdder'

export default {
  name: 'App',
  components: {
    TheHeader,
    TaskList,
    TaskAdder,
  },
  data(){
    return {
      tasks: [],
      showAddTask: false,
    }
  },
  methods:{
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },

    addTask(task){
      this.task = [...this.tasks, task]
    },
    deleteTask(id){
      // console.log(id)
      if(confirm('Are You Sure ?')) { 
        this.tasks = this.tasks.filter((xxx)=> xxx.id !== id)
      } 
    },
    toggleReminder(id){
      // console.log(id)
      this.tasks = this.tasks.map( 
        (xxx) => xxx.id === id ? {...xxx, reminder: !xxx.reminder} : xxx  
      )
    },
  },
  created(){
    this.tasks = [
      {
        id:1,
        text: 'Subuhan',
        day: 'March 1st at 4:30am',
        reminder: true
      },
      {
        id:2,
        text: 'Dzuhur',
        day: 'March 1st at 11:30am',
        reminder: true
      },
      {
        id:3,
        text: 'Ashar',
        day: 'March 1st at 3:01pm',
        reminder: false
      },
      {
        id:4,
        text: 'Maghrib',
        day: 'March 1st at 5:45pm',
        reminder: true
      },
      {
        id:5,
        text: 'Isya',
        day: 'March 1st at 7:30pm',
        reminder: true
      },


      
    ]
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
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
