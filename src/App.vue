<template>
  <div id="app">
    <div class="bg-primary mb-4">
      <div class="container">
        <div class="row justify-content-center todo-nav text-center">
          <div class="col-6">
            <div class="row">
              <div class="col-4">
                <a href="#"
                  :class="{'active': visibility == 'all'}"
                  @click="visibility = 'all'">My Tasks
                </a>
              </div>
              <div class="col-4">
                <a href="#"
                  :class="{'active': visibility == 'progress'}"
                  @click="visibility = 'progress'">In Progress
                </a>
              </div>
              <div class="col-4">
                <a href="#"
                  :class="{'active': visibility == 'completed'}"
                  @click="visibility = 'completed'">Completed
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row justify-content-center mb-3">
        <div class="col-6">
          <div class="position-relative d-flex align-items-center"
            v-if="isNewTodo === false && visibility == 'all'">
            <i class="fas fa-plus fa-lg text-black-50 position-absolute ml-3"></i>
            <input type="text" class="form-control form-control-lg" style="text-align:right;"
              @click="isNewTodo = true">
          </div>
        </div>
      </div>
      <edit-todo-item
        v-if="isNewTodo === true"
        @closeEditTodo="closeEdit"
      ></edit-todo-item>
      <draggable v-model="todos"
        @end="dragItem" :options="{handle: '.handle'}">
        <todo-item :todo="item"
          :filterTask="visibility"
          v-for="item in todos"
          :key="item.id"
          @updateData="getData">
        </todo-item>
      </draggable>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';
import TodoItem from './components/TodoItem.vue';
import EditTodoItem from './components/EditTodoItem.vue';

export default {
  name: 'App',
  components: {
    TodoItem,
    EditTodoItem,
    draggable,
  },
  data() {
    return {
      todos: [],
      isNewTodo: false,
      sortData: [],
      visibility: 'all',
    };
  },
  methods: {
    closeEdit() {
      this.isNewTodo = false;
      this.getData();
    },
    getData() {
      const vm = this;
      const api = 'http://localhost:3000/todos';
      vm.$http.get(api).then((response) => {
        vm.todos = response.data;
        return vm.$http.get('http://localhost:3000/sort');
      }).then((response) => {
        const sortData = response.data.sort;
        console.log(sortData);
      }).then((response) => {
        console.log(response);
      });
    },
    dragItem() {
      const vm = this;
      const api = 'http://localhost:3000/sort';
      const sorts = vm.todos.map((item) => {
        console.log(item);
        return item.id;
      });
      vm.$http.post(api, { sort: sorts }).then((response) => {
        console.log(response);
      });
      console.log(sorts);
    },
  },
  created() {
    this.getData();
  },
};
</script>

<style lang="scss">

</style>
