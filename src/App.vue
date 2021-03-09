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
    async addTask(task){
      // this.tasks = [...this.tasks, task]
      const res = await fetch('api/tasks',{
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(task)
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id){ 
      if(confirm('Are You Sure ?')) { 
        // this.tasks = this.tasks.filter((xxx)=> xxx.id !== id)
        const res = await fetch(`api/tasks/${id}`,{
        method: 'DELETE',
        // headers: { 'Content-Type': 'application/json' },
        })
      res.status === 200 ? (this.tasks = this.tasks.filter((xxx)=> xxx.id !== id)) : alert('Error Deleting Task...')
      } 
    },
    async toggleReminder(id){
        // console.log(id)
      const taskToToggle = await this.fetchTask(id)
      const updateTask = {...this.taskToToggle, reminder: !taskToToggle.reminder}
      const res = await fetch(`api/tasks/${id}`,{
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(updateTask)
        })
        const data = await res.json() 
        this.tasks = this.tasks.map( 
          (xxx) => xxx.id === id ? {...xxx, reminder: data.reminder} : xxx  
        ) 
        // this.tasks = this.tasks.map( 
        //   (xxx) => xxx.id === id ? {...xxx, reminder: !xxx.reminder} : xxx  
        //)
    },
    async fetchAllTasks(){
      const res = await fetch("api/tasks")
      const data = await res.json()
      return data
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    }
  },
  async created(){
    this.tasks = await this.fetchAllTasks()
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
