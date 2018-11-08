<template>
  <div id="app">

    <div class="container">

      <div class="col-6 md-offset-4">

        <div class="mb-3">
          <button>My Task</button>
          <button>In Progress</button>
          <button>Completed</button>
        </div>

        <task-form
          class="mb-3"
          @submit="addTask"
          submitText="Add Task">
        </task-form>

        <h3>Items</h3>

        <div class="items">
          <task-item
            v-for="item in items"
            v-bind:key="item.id"
            :item="item"
            :editingItem="editingItem"
            :editTask="editTask"
            :updateTask="updateTask"
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
      editingItem: '',
      form: {
        title: '',
      },
      items: [
        {
          id: 1,
          title: 'a',
        },
        {
          id: 2,
          title: 'b',
        },
      ]
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
    editTask (item) {
      this.editingItem = item
    },
    updateTask (form) {
      this.editingItem.title = form.title
      this.editingItem = null
    },
    cancelEditing () {
      this.editingItem = null
    },
    clearForm () {
      this.form.title = ''
    }
  }
}
</script>