<template>
  <div class="container">
    <h1>오늘의 할일</h1>
    <input v-model="searchText" type="text" class="form-control" placeholder="검색어를 입력하세요" />
    <TodoBasicForm @add-todo="onSubmit" />
    <div v-if="!todos.length">등록된 일정이 없습니다</div>
    <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
  </div>
</template>

<script>
import { ref, computed } from "vue";
import TodoBasicForm from "./components/TodoBasicForm.vue";
import TodoList from "./components/TodoList.vue";
export default {
  components: {
    TodoBasicForm,
    TodoList,
  },
  setup() {
    const toggle = ref(false);
    const searchText = ref("");
    const filteredTodos = computed(() => {
      //입력만 되면 ture
      if (searchText.value === "") {
        return todos.value.filter((todo) => {
          console.log(todos.value, todo);
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    });
    const todos = ref([]);
    const onSubmit = (todo) => {
      todos.value.push(todo);
    };
    const todoStyle = {
      textDecoration: "line-through",
      color: "gray",
    };
    const deleteTodo = (index) => {
      console.log(index);
      todos.value.splice(index, 1);
    };
    const toggleTodo = (index) => {
      console.log(index);
      todos.value[index].completed = !todos.value[index].completed;
    };

    return {
      filteredTodos,
      searchText,
      onSubmit,
      todos,
      toggle,
      todoStyle,
      deleteTodo,
      toggleTodo,
    };
  },
};
</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}
</style>