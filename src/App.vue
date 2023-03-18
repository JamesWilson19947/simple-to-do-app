<template>
  <div class="container mt-5">
    <h1 class="text-center mb-5">Simple To-Do</h1>
    <div class="row">
      <div class="col-md-8">
        <div class="mb-3">
          <input type="text" class="form-control" v-model="newTodo" placeholder="Add new todo" @keyup.enter="addTodo">
          <button class="btn btn-primary mt-3" @click="addTodo">Add Todo</button>
        </div>
        <ul class="list-group" ref="listGroup">
          <h3 class="mt-5">To-do:</h3>
          <li v-for="(todo, index) in incompleteTodos" :key="index" class="list-group-item">
            <div class="d-flex justify-content-between">
              <div class="form-check">
                <input class="form-check-input" type="checkbox" :id="'todo-' + index" v-model="todo.completed">
                <label class="form-check-label" :for="'todo-' + index">{{ todo.title }}</label>
              </div>
              <div class="d-flex">
                <button class="btn btn-danger" @click="removeTodo(index)"><font-awesome-icon
                    icon="fa-solid fa-circle-xmark" /></button>
                <div class="btn-group-vertical ms-2">
                  <button class="btn btn-secondary" @click="moveTodo(index, -1)" :disabled="index === 0">
                    <font-awesome-icon icon="fa-solid fa-arrow-up" />

                  </button>
                  <button class="btn btn-secondary" @click="moveTodo(index, 1)" :disabled="index === todos.length - 1">
                    <font-awesome-icon icon="fa-solid fa-arrow-down" />
                  </button>
                </div>
              </div>
            </div>
          </li>
        </ul>
        <ul class="list-group" ref="completedListGroup">
          <h3 class="mt-5" v-if="completedTodos.length > 0">Done:</h3>
          <li v-for="(todo, index) in completedTodos" :key="index" class="list-group-item"
            :class="{ completed: todo.completed }">
            <div class="d-flex justify-content-between">
              <div class="form-check">
                <input class="form-check-input" type="checkbox" :id="'todo-' + index" v-model="todo.completed">
                <label class="form-check-label" :for="'todo-' + index">{{ todo.title }}</label>
              </div>
              <div class="d-flex">
                <button class="btn btn-danger" @click="removeTodo(index)"><font-awesome-icon
                    icon="fa-solid fa-circle-xmark" /></button>
                <div class="btn-group-vertical ms-2">
                  <button class="btn btn-secondary" @click="moveTodo(index, -1)" :disabled="index === 0">
                    <font-awesome-icon icon="fa-solid fa-arrow-up" />

                  </button>
                  <button class="btn btn-secondary" @click="moveTodo(index, 1)" :disabled="index === todos.length - 1">
                    <font-awesome-icon icon="fa-solid fa-arrow-down" />
                  </button>
                </div>
              </div>
            </div>
          </li>
        </ul>

      </div>
      <div class="col-md-4">
        <h2 class="mb-4">About this tool</h2>
        <p>This to-do is stored in local browser storage, you clear your cache and its gone!</p>
        <p>To add a new todo, enter the title in the input field and click the "Add Todo" button.</p>
        <p>To mark a todo as completed, click the checkbox next to the todo's title.</p>
        <p>To remove a todo, click the "Remove" button next to the todo's title.</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, watch, computed } from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const todos = ref(JSON.parse(localStorage.getItem('todos') || '[]'));

    watch(todos, (newValue) => {
      localStorage.setItem('todos', JSON.stringify(newValue));
    }, { deep: true });
    const incompleteTodos = computed(() => {
      return todos.value.filter(todo => !todo.completed);
    });
    const completedTodos = computed(() => {
      return todos.value.filter(todo => todo.completed);
    });
    const addTodo = () => {
      if (newTodo.value.trim() === '') {
        return;
      }
      todos.value.push({
        title: newTodo.value,
        completed: false
      });
      newTodo.value = '';
    };

    const removeTodo = (index) => {
      todos.value.splice(index, 1);
    };

    const moveTodo = (index, direction) => {
      const newIndex = index + direction;
      const tempTodo = todos.value[index];
      todos.value.splice(index, 1);
      todos.value.splice(newIndex, 0, tempTodo);
    };

    return { newTodo, todos, addTodo, removeTodo, moveTodo, incompleteTodos, completedTodos };
  },
};
</script>

<style>
li.list-group-item {
  cursor: pointer;
}

.completed {
  background-color: #e6e6e6 !important;
}</style>
