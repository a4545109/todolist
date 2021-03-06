<template>
  <div class="row justify-content-center"
      v-if="filterTask === 'all'
      || filterTask === 'progress' && todo.completed === false
      || filterTask === 'completed' && todo.completed === true">
    <div class="col-6">
      <div class="mb-4">
        <div class="todo-item d-flex align-items-center" :class="{stared: todo.stared}">
          <div class="handle text-muted position-relative">
            <i class="fas fa-ellipsis-v position-absolute" style="left: 5px;"></i>
          </div>
          <div class="w-100">
            <div class="w-100">
              <div class="todo-header d-flex justify-content-between">
                  <div class="todo-check" v-if="todo.open == false">
                    <input type="checkbox"
                      class="form-check-input"
                      v-model="todo.completed"
                      :id="todo.id"
                      @click.prevent="completedTodo">
                  </div>
                  <div class="todo-title">
                    <input type="text" value="可輸入內容" class="form-control"
                        v-model="todo.message"
                        v-if="todo.open == true">
                    <label class="form-check-label"
                      v-if="todo.open == false"
                      :class="{'completed': todo.completed}"
                      :for="todo.id">
                        {{ todo.message }}
                    </label>
                  </div>
                  <div class="todo-control" v-if="todo.open == false">
                    <a href="#" class="text-muted" @click.prevent="updateStar">
                      <i class="far fa-star mr-3"
                        v-if="todo.stared == false"></i>
                      <i class="fas fa-star mr-3"
                        v-if="todo.stared == true"></i>
                    </a>
                    <a href="#" class="text-muted mr-3" @click.prevent="editTodoClose">
                      <i class="fas fa-pencil-alt"></i>
                    </a>
                    <a href="#" class="text-muted" aria-label="Close"
                      @click.prevent="removeItem(todo)">
                      <i class="fas fa-times-circle"></i>
                    </a>
                  </div>
              </div>
            </div>
            <div class="horizontalLine" v-if="todo.open == true"></div>
            <div class="todo-footer text-secondary py-2" v-if="todo.open == false">
              <i class="far fa-calendar-alt mr-1"></i>
              <span>from:{{ todo.startDate }}</span>
              <span> ~ </span>
              <span>to: {{ todo.endDate }} </span>
              <i class="far fa-file ml-3"></i>
              <i class="fas fa-comment-dots mr-1 ml-3"></i>
            </div>
            <div class="todo-body" v-if="todo.open == true">
              <div class="todo-content">
                <div class="todo-row">
                  <div class="mb-2">
                    <i class="far fa-calendar-alt mr-1"></i><span>Deadline</span>
                  </div>
                  <div class="form-inline mb-3">
                    <input type="date" class="form-control border-0" v-model="todo.startDate">
                    <input type="date" class="form-control border-0 ml-2" v-model="todo.endDate">
                  </div>
                  <div class="mb-3">
                    <i class="far fa-file mr-1"></i><span>File</span>
                  </div>
                  <div class="mb-3">
                    <i class="far fa-comment-dots mr-1"></i><span>Comment</span>
                  </div>
                  <div>
                    <textarea name="" class="form-control"
                      placeholder="Type your memo here…" v-model="todo.comments"></textarea>
                  </div>
                </div>
              </div>
              <div class="todo-btn-group btn-group d-flex">
                <button class="btn btn-light text-danger" @click.prevent="editTodoClose">
                  <i class="fas fa-times"></i>Cancel
                </button>
                <button class="btn btn-primary" @click.prevent="editTodoClose">
                  <i class="fas fa-plus"></i>Add Task
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todo', 'filterTask'],
  name: 'Todo',
  methods: {
    updateStar() {
      const vm = this;
      const api = `http://localhost:3000/todos/${vm.todo.id}`;
      const todo = {
        ...vm.todo,
      };
      todo.stared = !vm.todo.stared;
      vm.$http.put(api, todo).then((response) => {
        console.log(response);
        vm.$emit('updateData');
      });
    },
    editTodoClose() {
      const vm = this;
      const api = `http://localhost:3000/todos/${vm.todo.id}`;
      const todo = {
        ...vm.todo,
      };
      todo.open = !vm.todo.open;
      vm.$http.put(api, todo).then((response) => {
        console.log(response);
        vm.$emit('updateData');
      });
    },
    removeItem(todo) {
      const vm = this;
      const api = `http://localhost:3000/todos/${vm.todo.id}`;
      vm.$http.delete(api, todo).then((response) => {
        console.log(response);
        vm.$emit('updateData');
      });
    },
    completedTodo() {
      const vm = this;
      const api = `http://localhost:3000/todos/${vm.todo.id}`;
      const todo = {
        ...vm.todo,
      };
      todo.completed = !vm.todo.completed;
      vm.$http.put(api, todo).then((response) => {
        console.log(response);
        vm.$emit('updateData');
      });
    },
  },
};
</script>
