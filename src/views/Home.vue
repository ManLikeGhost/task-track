<template>
    <AddTask 
        v-show="displayAddTask" 
        @add-new-task="addNewTask" 
    />
    <Tasks 
        @toggle-reminder="toggleReminder" 
        @del-task="delTask" 
        :tasks= "tasks" 
    />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
    name: 'Home',

    props: {
        displayAddTask: Boolean,
    },

  components: {
      Tasks,
      AddTask
  },

  data() {
    return{
        tasks: [],
    }
  },

  methods: {
    async addNewTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },

    async delTask(id) {
      if(confirm('Are you certain you want to delete ? ')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        })

        res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Something went wrong when deleting!!')
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id) 

      const updateTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updateTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map((task) => 
        task.id === id ? {...task, reminder: data.reminder} : task )
    },

    async fetchTasks() {
      const res = await fetch('api/tasks')

      const data = await res.json()

      return data
    },

    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data
    },

    async created() {
        this.tasks = await this.fetchTasks()
    },
  },
    
}
</script>