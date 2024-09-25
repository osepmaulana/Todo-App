<template>
  <div
    :class="{ 
      'app-container container d-flex align-items-center justify-content-center flex-column': visible, 
      'd-none': !visible 
    }"
  >
    <h3 class="mb-4 text-dark font-weight-bold">Todo App</h3>

    <div class="d-flex align-items-center mb-3 w-100">
      <div class="form-group w-75 mb-0">
        <input
          type="text"
          id="task"
          class="form-control shadow-sm"
          placeholder="Enter a task"
          v-model="form.name"
          @keyup.enter="onSave"
          autofocus
        />
      </div>
      <button
        type="button"
        class="btn btn-primary ml-2 px-4 shadow-sm"
        @click="onSave"
      >
        Save
      </button>
      <button 
        type="button" 
        class="btn btn-outline-secondary ml-2 px-4 shadow-sm"
        @click="onClear"
      >
        Clear
      </button>
    </div>

    <div class="top-left-alert">
      <div 
        class="alert alert-success alert-dismissible fade shadow-sm" 
        :class="{ 'show': success, 'd-none': !success }"
        role="alert"
      >
        Task successfully added.
        <button 
          type="button" 
          class="close" 
          aria-label="Close"
          @click="onCloseAlert"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
    </div>

    <div class="table-wrapper w-100 mt-4">
      <table class="table table-hover table-bordered shadow-sm">
        <thead class="thead-dark">
          <tr>
            <th>No.</th>
            <th>Todo Item</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(task, index) in tasks"
            :key="index"
          >
            <td>{{ index + 1 }}</td>
            <td>{{ task.name }}</td>
            <td>
              <button 
                class="btn btn-danger btn-sm px-3"
                @click="onDelete(index)"
              >
                Delete
              </button>
            </td>
          </tr>
          <tr v-if="tasks.length === 0">
            <td colspan="3" class="text-center text-muted">No tasks available</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data: () => ({
    form: {
      name: null
    },
    tasks: [],
    success: false
  }),
  methods: {
    onSave() {
      let name = this.form.name
      if (!name) return 
      this.tasks.push({ name })
      this.$nextTick(() => {
        this.form.name = null
        this.success = true
        setTimeout(() => {
          this.success = false
        }, 2000) 
      })
    },
    onDelete(index) {
      this.tasks.splice(index, 1)
    },
    onClear() {
      this.tasks = []
    },
    onCloseAlert() {
      this.success = false
    }
  }
}
</script>

<style scoped>
.app-container {
  width: 100%;
  max-width: 600px;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h3 {
  color: #343a40;
  font-family: 'Helvetica Neue', sans-serif;
}

.form-control {
  border: 1px solid #ced4da;
  border-radius: 6px;
  padding: 10px;
}

.table-wrapper {
  margin-top: 20px;
}

thead th {
  background-color: #343a40;
  color: #fff;
}

tbody tr:hover {
  background-color: #f8f9fa;
}

.btn {
  min-width: 80px;
  transition: background-color 0.2s ease;
}

.btn:hover {
  background-color: #495057;
}

.alert {
  position: absolute;
  top: 20px;
  right: 20px;
  left: auto;
  width: auto;
}

.close {
  font-size: 1.2rem;
}

.shadow-sm {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.table {
  border-radius: 6px;
  overflow: hidden;
}

.table th, .table td {
  padding: 12px 16px;
}

thead th {
  font-weight: 600;
}

.thead-dark {
  background-color: #212529;
}
</style>
