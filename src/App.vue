<template>
  <div id="app">

    <div class="container">

      <div class="col-6 md-offset-4">

        <div class="mb-3">
          <button @click="filter('all')">My Task</button>
          <button @click="filter('in_progress')">In Progress</button>
          <button @click="filter('complete')">Completed</button>
        </div>

        <task-form
          class="mb-3"
          @submit="addTask"
          submitText="Add Task">
        </task-form>

        <h3>Items</h3>

        <div class="items">
          <task-item
            v-for="item in filtered"
            v-bind:key="item.id"
            :item="item"
            :editingItem="editingItem"
            :editTask="editTask"
            :updateTask="updateTask"
            :deleteTask="deleteTask"
            :toggleTask="toggleTask"
            :starTask="starTask"
            :cancelEditing="cancelEditing">
          </task-item>
        </div>

      </div>

    </div>


  </div>
</template>

<script>
import axios from 'axios'
import TaskForm from './components/TaskForm.vue'
import TaskItem from './components/TaskItem.vue'

export default {
  components: {
    TaskForm, TaskItem
  },
  name: 'app',
  data () {
    return {
      filteringBy: 'all',
      editingItem: '',
      form: {
        title: '',
        completed: false,
        stared: false
      },
      items: []
    }
  },
  mounted () {
    // call api to get task lists
    axios.get('http://localhost:3000/task')
      .then((response) => {
        this.items = response.data
      })
  },
  computed: {
    filtered () {
      let items = []
      switch (this.filteringBy) {
        case 'all':
          return this.items
        case 'complete':
          for (let item of this.items) {
            if (item.completed) {
              items.push(item)
            }
          }
          return items
        case 'in_progress':
          for (let item of this.items) {
            if (!item.completed) {
              items.push(item)
            }
          }
          return items
      }
    }
  },
  methods: {
    addTask (form) {
      // 1. copy form data
      let cloned = Object.assign({} , form)
      cloned.id = Date.now()
      cloned.created_at = Date.now()
      // 2. push into items
      this.items.unshift(cloned)
      // 3. clear origin form
      this.clearForm()

      // 4. save to json server
      axios.post('http://localhost:3000/task', cloned)
    },
    starTask (item) {
      item.stared = !item.stared
      if (item.stared) {
        this.items.splice(this.items.indexOf(item), 1)
        this.items.unshift(item)
      } else {
        this.items.splice(this.items.indexOf(item), 1)

        let lastStarPosition = -1
        for (let i = 0; i < this.items.length; i++) {
          if (this.items[i].stared) {
            lastStarPosition = i
          }
        }

        let position = lastStarPosition
        for (let i = lastStarPosition + 1; i < this.items.length; i++) {
          if (item.created_at < this.items[i].created_at) {
            position = i
          }
        }
        this.items.splice(position + 1, 0, item)
      }
    },
    toggleTask (item) {
      item.completed = !item.completed
      axios.patch(`http://localhost:3000/task/${item.id}`, item)
    },
    editTask (item) {
      this.editingItem = item
    },
    updateTask (form) {
      this.editingItem.title = form.title
      axios.patch(`http://localhost:3000/task/${this.editingItem.id}`, this.editingItem)
      this.editingItem = null
    },
    deleteTask (item) {
      this.items.splice(this.items.indexOf(item), 1)
      axios.delete(`http://localhost:3000/task/${item.id}`)
    },
    cancelEditing () {
      this.editingItem = null
    },
    clearForm () {
      this.form.title = ''
    },
    filter (type) {
      this.filteringBy = type
    }
  }
}
</script>