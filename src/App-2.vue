<!-- Built-in Directives => vue에 내장되어있는 지시문 v- 로 시작 -->
<template>
  <div>
    <h1 :class="color">{{ name }}</h1>
  </div>
  <input type="text" :value="name">
  <!--  setup안의 데이터를 연결할때는 v-bind 를 사용하여 연결 :줄여쓰기 -->
  <button class="btn btn-primary" @:click="updateName">버튼</button>
  <!-- v-on 이벤트연결 @줄임말 -->
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    let name = ref('망고');
    let color = ref('red');
    console.log(color);

    const updateName = () => {
      name.value = '회원님';
      color.value = 'green';
    }
    return { name, color, updateName };
  }
}
</script>

<style>
.red {
  background-color: red;
}

.green {
  background-color: green;
}
</style>