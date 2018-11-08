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
      },
      items: [
        {
          id: 1,
          title: 'a',
          completed: false,
          stared: false
        },
        {
          id: 2,
          title: 'b',
          completed: false,
          stared: false
        },
      ]
    }
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
      // 2. push into items
      this.items.unshift(cloned)
      // 3. clear origin form
      this.clearForm()
    },
    starTask (item) {
      item.stared = !item.stared
    },
    toggleTask (item) {
      item.completed = !item.completed
    },
    editTask (item) {
      this.editingItem = item
    },
    updateTask (form) {
      this.editingItem.title = form.title
      this.editingItem = null
    },
    deleteTask (item) {
      this.items.splice(this.items.indexOf(item), 1)
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