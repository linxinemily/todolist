<template>
  <div id="app">

    <div class="container">

      <div class="col-6 md-offset-4">

        <div class="mb-3">
          <button>My Task</button>
          <button>In Progress</button>
          <button>Completed</button>
        </div>

        <div class="card mb-3">
          <div class="card-body">
            <input v-model="form.title" class="form-control" type="text">
          </div>
          <div class="card-footer">
            <button>Cancel</button>
            <button @click="addTask()">Add Task</button>
          </div>
        </div>

        <h3>Items</h3>

        <div class="items">
          <div class="item mb-3"
            v-for="item in items"
            v-bind:key="item.id">
            <!-- normal -->
            <div class="card" v-if="editingItem !== item">
              <div class="card-body" >
                <div class="float-left">{{ item.title }}</div>
                <button @click="editTask(item)" class="btn btn-primary float-right">Edit</button>
              </div>
            </div>
            <!-- end normal -->

            <!-- editing -->
            <div class="card" v-else>
              <div class="card-body">
                <input v-model="form.title" class="form-control" type="text">
              </div>
              <div class="card-footer">
                <button>Cancel</button>
                <button @click="addTask()">Add Task</button>
              </div>
            </div>
            <!-- end editing -->

          </div>
        </div>

      </div>

    </div>


  </div>
</template>

<script>
export default {
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
    addTask () {
      // 1. copy form data
      let cloned = Object.assign({} , this.form)
      // 2. push into items
      this.items.unshift(cloned)
      // 3. clear origin form
      this.clearForm()
    },
    editTask (item) {
      this.editingItem = item
    },
    clearForm () {
      this.form.title = ''
    }
  }
}
</script>