<template>
  <!-- view -->
  <div class="container">
    <h1>오늘의 할일</h1>

    <TodoBasicForm @add-todo="onAdd" />
    <!-- TodoBasicForm에 있는 add-todo를 연결 -->
    <TodoList :todos="todos" @toggle-todo="toggleTodo" />

    <div v-if="!todos.length">등록된 일정이 없습니다.</div>

  </div>
</template>

<script>
import { ref } from "vue";
import TodoBasicForm from './components/TodoBasicForm.vue';
import TodoList from './components/TodoList.vue'

export default {
  components: { TodoBasicForm, TodoList },

  setup() {
    const todos = ref([]);

    const deleteTodo = (data) => {
      console.log(data);
      todos.value.splice(data, 1)
    }
    const toggleTodo = (index) => {
      // console.log(index);
      todos.value[index].completed = !todos.value[index].completed
    }
    const onAdd = (todo) => {
      // console.log(data);
      todos.value.push(todo)
    }


    return { onAdd, deleteTodo, todos, toggleTodo };
  },
};
</script>
<style>
.red {
  color: red;
}

.todo {
  color: gray;
  text-decoration: line-through;
}
</style>