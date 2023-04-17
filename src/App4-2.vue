<template>
  <div class="container">
    <h1>오늘의 할일 (좀 하자 ...)</h1>
    <TodoBasicForm />

    <div>
      <!-- v-else-if 조건이 여러개 -->
      <div v-if="toggle">🤢🤢🤢🤢🤢🤢🤢🤢</div><!-- v-if : true인 상황에 실행 =>ifToggle는 초기값이 false -->
      <!-- <div v-if="!ifToggle">🤡</div> -->
      <div v-else>🤡🤡🤡🤡🤡🤡🤡🤡🤡</div><!-- whrjsdl djqtdma -->
      <button class="btn btn-warning" type="submit" @:click="onToggle">토글</button>
    </div>
    <form action="#" class="d-flex" @:submit.prevent="onSubmit">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="일정 추가하기" v-model="todo">
        <button class="btn btn-primary" type="submit">추가</button>
      </div>
      <div v-if="errMsg" class="alert alert-danger">할일을 입력해주세요</div>
    </form>

    <div v-if="!todos.length">등록된 일정이 없습니다.</div>

    <!-- <div class="card">
      <div class="card-body p-2">
        {{ todos[0].subject }}
      </div>
    </div> -->
    <div class="card" v-for="i in todos" :key="i.id">
      <div class="card-body p-2">
        <div class="d-flex">
          <!-- .form-check>label.form-check-label+input.form-check-input -->
          <div class="form-check flex-grow-1">
            <!-- 클래스와 바인딩하려면 setup안에 작성되어야 함 -->
            <!-- <label class="form-check-label" :class={todo:i.completed}> //class로 바인딩하는것 => todo라는 클래스를 i.completed 에 바인딩 하겠다. -->
            <label class="form-check-label" :style="i.completed ? todoStyle : {}">
              <!-- input 태그에는 checked 속성이 들어있어서 checked가 되면 true, 아니면 false -->
              <input v-model="i.completed" type="checkbox" class="form-check-input">
              <!-- v-model 연결 -->
              {{ i.subject }} </label>
          </div>
          <div><button class="btn btn-danger btn-sm" @click="deleteTodo(data)">삭제</button></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import TodoBasicForm from './components/TodoBasicForm.vue';

export default {
  components: { TodoBasicForm },
  setup() {
    const todo = ref("");
    const toggle = ref(false);
    const errMsg = ref(false);
    const todos = ref([]);
    /* let todos = ref([
      {
        id: "1",
        subject: "리액트복습",
      },
      {
        id: "2",
        subject: "냥빨",
      }
    ]); */



    const onSubmit = () => {
      if (todo.value === false) {
        errMsg.value = true
      } else {
        // console.log(todo.value);
        //배열에 push
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: false,
        })
        /* 초기화 */
        todo.value = "";
        errMsg.value = false;

      }
      console.log(todos.value);

    };
    const onToggle = () => {
      toggle.value = !toggle.value
    }
    const todoStyle = {
      color: 'gray',
      'text-decoration': 'line-through',

    }
    const deleteTodo = (data) => {
      console.log(data);
      todos.value.splice(data, 1)
      //시작번호로 부터 한개를 지우겠다.
    }
    return {
      todo, onSubmit, todos, onToggle, toggle, errMsg, todoStyle, deleteTodo,
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