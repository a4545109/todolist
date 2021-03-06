<template>
  <div class="row justify-content-center my-3">
    <div class="col-6">
      <div>
        <div class="todo-item">
          <div class="todo-header d-flex justify-content-between">
            <div class="w-100">
              <div class="todo-title">
                <input type="text" value="可輸入內容" class="form-control"
                  v-model="cacheTodo.message">
              </div>
            </div>
          </div>
          <div class="horizontalLine"></div>
          <div class="todo-body">
            <div class="todo-content">
              <div class="todo-row">
                <div class="mb-2">
                  <i class="far fa-calendar-alt mr-1"></i><span>Deadline</span>
                </div>
                <div class="form-inline mb-3">
                  <input type="date" class="form-control border-0" v-model="cacheTodo.startDate">
                  <input type="date" class="form-control border-0 ml-2" v-model="cacheTodo.endDate">
                </div>
                <div class="mb-3">
                  <i class="far fa-file mr-1"></i><span>File</span>
                </div>
                <div class="mb-3">
                  <i class="far fa-comment-dots mr-1"></i><span>Comment</span>
                </div>
                <div>
                  <textarea name="" class="form-control"
                    placeholder="Type your memo here…" v-model="cacheTodo.comments"></textarea>
                </div>
              </div>
            </div>
            <div class="todo-btn-group btn-group d-flex">
              <button class="btn btn-light text-danger" @click="closeTodo">
                <i class="fas fa-times"></i>Cancel
              </button>
              <button class="btn btn-primary" @click="updateTodo">
                <i class="fas fa-plus"></i>Add Task
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  name: 'EditTodo',
  data() {
    return {
      cacheTodo: {},
    };
  },
  methods: {
    updateTodo() {
      const vm = this;
      const api = 'http://localhost:3000/todos';
      const todo = {
        ...vm.cacheTodo,
        stared: false,
        completed: false,
        open: false,
      };
      vm.$http.post(api, todo).then((response) => {
        console.log(response);
        vm.closeTodo();
      });
    },
    closeTodo() {
      this.$emit('closeEditTodo');
      // $emit 會觸發外層的事件
    },
  },
};
</script>
