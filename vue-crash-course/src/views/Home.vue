<template>
      <AddTask
        v-show="showAddTask"
        @add-task="addTask" 
      />
    <Tasks 
      @delete-task="deleteTask" 
      :tasks="tasks"
      @toggle-reminder="toggleReminder"
    />
</template>s

<script>
    import Tasks from '../components/Tasks.vue';
    import AddTask from '../components/AddTask.vue';

    export default {
        name: 'Home',
        props: {
            showAddTask: {
                type: Boolean
            }
        },
        components: {
            Tasks,
            AddTask
        },
        data() {
            return {
                tasks: [],
            }
        },
        methods: {
            async addTask(task) {

                const res  = await fetch('api/tasks', {
                    method: 'POST',
                    headers: {
                    'Content-type' : 'application/json',
                    },
                    body: JSON.stringify(task),
                });

                const data = await res.json();
                this.tasks = [...this.tasks, data];
            },

            async deleteTask(id) {
            
                if (confirm("Are you sure?")) {
                    const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                    });

                    if (res.status === 200) {
                        this.tasks = this.tasks.filter((task) => task.id !== id)
                    } else {
                        alert("Error deleting task!");
                    }
                }
            },

            async toggleReminder(id) {
                const taskToggle = await this.fetchTask(id);
                const upTask = {...taskToggle, reminder: !taskToggle.reminder};
                const res = await fetch(`api/tasks/${id}`, {
                    method: 'PUT',
                    headers: {
                        'Content-type' : 'application/json'
                    },
                    body: JSON.stringify(upTask),
                });

                const data = res.json();

                this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: !task.reminder} : task)
            },
            
            async fetchTasks() {
                const res = await fetch("api/tasks");

                const data = await res.json();

                return data;
            },

            async fetchTask(id) {
                const res = await fetch(`api/tasks/${id}`);

                const data = await res.json();

                return data;
            },
        },
        async created() {
            this.tasks = await this.fetchTasks();
        }
    }
</script>