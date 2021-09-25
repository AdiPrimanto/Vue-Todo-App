<template>
  <div class="container">
    <h2 class="text-center mt-5">Vue To Do App</h2>

    <!-- input -->
    <div class="d-flex">
      <input type="text" v-model="task" placeholder="Enter text" class="form-control">
      <button class="btn btn-warning rounded-0" @click="submitTask">Submit</button>
    </div>

    <!-- Task table -->
    <div class="table-responsive mt-5">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col" class="text-center">#</th>
            <th scope="col" class="text-center">#</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>
              <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span>
            </td>
            <td style="width: 120px">
              <span class="pointer" @click="changeStatus(index)" 
              :class="{'text-danger': task.status === 'to-do', 'text-primary': task.status === 'in-progress', 'text-success': task.status === 'finished'}">
                {{ firstCharUpper(task.status) }}
              </span>
            </td>
            <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen"></span>
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ToDoApp',
  data() {
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Steal bananas from store',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour',
          status: 'in-progress'
        }
      ]
    }
  },
  methods: {
    submitTask() {
      if(this.task.length === 0) return;

      if(this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null
      }

      this.task = '';
    },
    deleteTask(id) {
      this.tasks.splice(id, 1);
    },
    editTask(id) {
      this.task = this.tasks[id].name;
      this.editedTask = id;
    },
    changeStatus(id) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[id].status);
      console.log('cek index', newIndex);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[id].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>
