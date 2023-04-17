<template>
  <div class="container">
    <h4>count:{{ count }}</h4>
    <h4>double Count:{{ doubleCountComputed }}</h4>
    <h4>double Function:{{ doubleCountFn() }}</h4>
    <h4>double Count:{{ doubleCountComputed }}</h4>
    <h4>double Function:{{ doubleCountFn() }}</h4>
    <h4>double Count:{{ doubleCountComputed }}</h4>
    <h4>double Function:{{ doubleCountFn() }}</h4>
    <button @click="count++">add</button>
    <h1>오늘의 할일</h1>
    <TodoBasicForm @add-todo="onAdd" />
    <TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
    <div v-if="!todos.length">등록된 일정이 없습니다.</div>
  </div>
</template>

<script>
/* computed 인스턴스의 데이터를 캐싱(저장)할때 쓴다 */
import { ref, computed } from "vue";
import TodoBasicForm from "./components/TodoBasicForm.vue";
import TodoList from "./components/TodoList.vue";
export default {
  components: { TodoBasicForm, TodoList },
  setup() {
    let todos = ref([]);

    const count = ref(1);
    const doubleCountComputed = computed(() => {
      console.log(computed);
      return count.value * 2
    });
    const doubleCountFn = () => {
      console.log("함수");
      return count.value * 2
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };
    const toggleTodo = (index) => {
      console.log("eeee", todos[index]);
      todos.value[index].completed = !todos.value[index].completed
      console.log('😚😚', todos.value[index].completed);
    };
    const onAdd = (todo) => {
      todos.value.push(todo);
    };

    return {
      toggleTodo,
      onAdd,
      deleteTodo,
      todos,
      count,
      doubleCountComputed, doubleCountFn
    };
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