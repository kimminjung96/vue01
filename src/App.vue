<template>
  <div class="container">
    <h1>오늘의 할일</h1>
    <input v-model="searchText" type="text" class="form-control" placeholder="검색어를 입력하세요" />
    <TodoBasicForm @add-todo="onSubmit" />
    <div style="color:red">{{ error }}</div>
    <div v-if="!todos.length">등록된 일정이 없습니다</div>
    <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
    <!-- <nav aria-label="Page navigation example"> =>접근성레이블 : 접근성 -->
    <nav>
      <ul class="pagination justify-content-center">
        <li v-if="currentPage !== 1" class="page-item"><a class="page-link" href="#"
            @click="getTodos(currentPage - 1)">Previous</a></li>
        <li v-for="page in numberOfPages" class="page-item" :key="page" :class="currentPage === page ? `active` : ``"><a
            class="page-link" @click="getTodos(page)">{{ page }}</a></li>
        <li v-if="currentPage !== numberOfPages" class="page-item"><a class="page-link" href="#"
            @click="getTodos(currentPage + 1)">Next</a></li>
      </ul>
    </nav>
    <!-- {{ numberOfPages }} -->
  </div>
</template>

<script>
import { ref, computed } from "vue";
import axios from "axios";
import TodoBasicForm from "./components/TodoBasicForm.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: {
    TodoBasicForm,
    TodoList,
  },
  setup() {
    let error = ref("");
    const toggle = ref(false);
    const searchText = ref("");
    const todos = ref([]);
    const totalTodos = ref(0);
    /* 모든데이터 */
    const limit = 5;
    /* 보일데이터 */
    const currentPage = ref(1);
    /* 선택된 페이지 */

    const numberOfPages = computed(() => {
      return Math.ceil(totalTodos.value / limit)
    });


    const filteredTodos = computed(() => {
      console.log(searchText.value);
      if (searchText.value) {
        //입력만되면 ture
        return todos.value.filter((todo) => {
          console.log(todos.value, todo);
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    });


    const getTodos = (page = currentPage.value) => { //page 버튼주소
      currentPage.value = page; //age.value 페이지주소
      axios.get(`http://localhost:8080/todos?_page=${page}&_limit=${limit}`)
        .then((res) => {
          /* 객체의 속성에 특수문자가 포함되면 대괄호로 [] */
          console.log("이것은 todos.value입니다", res.headers["x-total-count"]);
          /* https://yamoo9.github.io/axios/guide/response-schema.html 응답시키마 */
          totalTodos.value = res.headers["x-total-count"];
          todos.value = res.data;

        })
        .catch((err) => { console.log(err); error.value = "getTodos 일시적으로 오류발생." })
    }
    getTodos();
    console.log("현재 todos", todos);
    const onSubmit = (todo) => {
      error.value = "";
      axios.post("http://localhost:8080/todos", {
        subject: todo.subject,
        completed: todo.completed,
      }).then((res) => {

        console.log(res.data.todos);
        todos.value.push(res.data.todos);
        getTodos();
        /* console.log(res.data.result);
        todos.value.push(res.data.result); */
        //두개 사용시 배열로
        /* return [
          console.log(res.data,"😀😀"),
          todos.value.push(res.data.result)
        ] */
      }).catch((err) => {
        "😋", console.log(err);
        error.value = "일시적으로 에러가 발생 잠시후 다시해주세요"
      })

    };
    /*     const todoStyle = {
          textDecoration: "line-through",
          color: "gray",
        }; */
    const deleteTodo = (index) => {
      error.value = "";
      // console.log(index);
      const id = index;
      axios.delete("http://localhost:8080/todos/" + id)
        .then(() => {
          // todos.value.splice(id, 1);
          getTodos();
        })
        .catch((err) => { console.log(err); })

    };

    /* fetch : 부분적 수정 ,put : 전체수정 */
    const toggleTodo = (index) => {
      const id = index;
      axios.post("http://localhost:8080/todos/" + id)
        .then((res) => {
          console.log("toggleTodo", res, toggle.value),
            getTodos();
        })
        .catch((err) => console.log(err));
    };
    // const toggleTodo = (index) => {
    //   const id = index;
    //   // axios.patch("http://localhost:3000/todos/" + id, {
    //   axios.post("http://localhost:3000/todos/" + id)
    //     .then((res) => {
    //       console.log(res);
    //       //todos.value.completed = !todos.value.completed 
    //     })
    //     .catch((err) => console.log(err));
    //   // console.log(index);
    //   // todos.value[index].completed = !todos.value[index].completed;
    // };

    //화면에 나타나야하는것만 return
    return {
      numberOfPages,
      currentPage,
      filteredTodos,
      searchText,
      onSubmit,
      todos,
      toggle,
      getTodos,
      deleteTodo,
      toggleTodo,
      error,
    };
  },
};
</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}

.page-item a {
  cursor: pointer;
}
</style>