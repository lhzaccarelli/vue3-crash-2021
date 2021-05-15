<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks :tasks="tasks" 
            @delete-task="deleteTask" 
            @toggle-reminder="toggleReminder"></Tasks>
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
    name: 'Home',
    components: {
        Tasks, AddTask
    },
    props: {
        showAddTask: Boolean
    },
    data() {
        return {
            tasks: [],
        }
    },
    methods: {
        async addTask(task) {
            const res = await fetch('api/tasks', {
            method: 'POST', 
            headers: {
                'Content-type' : 'application/json',
            },
            body: JSON.stringify(task)
            })

            const data = await res.json()

            this.tasks.push(data)
        },
        async deleteTask(id) {
            if (confirm('Are you sure?')) {
            const res = await fetch(`api/tasks/${id}`, {
                method: "DELETE"
            })

            if (res.status === 200) {
                this.tasks = this.tasks.filter((task) => task.id !== id)
            } else {
                alert('Error deleting task')
            }

            }
        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id)
            const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

            const res = await fetch(`api/tasks/${id}`, {
            method: "PUT",
            headers: { 
                "Content-type": 'application/json'
            },
            body: JSON.stringify(updTask)
            })

            const data = await res.json()

            this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task)
        },
        async fetchTasks() {
            const res = await fetch('api/tasks')
            return await res.json()
        },
        async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`)
            return await res.json()
        },
    },
    async created() {
        this.tasks = await this.fetchTasks()
    }
}
</script>