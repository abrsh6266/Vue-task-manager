<template>
      <AddTask v-show="showAddTask" @add-task="AddTask" />
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>
<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default{
    name: 'HomeView',
    components:{
        Tasks,
        AddTask,
    },
    data(){
        return {
            tasks:[],
        }
    },
    props:{
        showAddTask: Boolean
    },
    async created(){
    this.tasks = await this.fetchTasks()
    },
    methods:{
    async deleteTask(id){
      if(confirm('are you sure?')){
        const res = await fetch(`api/tasks/${id}`,{
          method:'DELETE',})

          res.status===200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : 
          alert('Error Deleting')
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const   updTask = {...taskToToggle, reminder:!taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`,{
        method: 'PUT',
        headers:{
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })
      this.tasks = this.tasks.map((task) => task.id === id ? {
        ...task, reminder: updTask.reminder} :task
      )
    },
    async AddTask(task){
      const res = await fetch('api/tasks',{
        method: 'POST',
        headers:{
          'Content-type':'application/json',
         
        },
        body: JSON.stringify(task),
      })

      const data = await res.json()
      
      this.tasks = [...this.tasks, data]
    },
    async fetchTasks(){
      const res = await fetch('api/tasks')
      const data = await res.json()
      
      return data
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      
      return data
    }
  },
}
</script>