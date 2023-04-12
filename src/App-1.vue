<template>
  <h1 class="app">{{ name }}App</h1>
  <!-- 오류남 => <button type="button" class="btn btn-primary" onClick="alert('sss')">클릭</button> -->
  <button type="button" class="btn btn-primary" v-on:click="updateName">클릭</button>
  <!-- v-on => js와 이어줌 -->
  <button type="button" class="btn btn-warning" v-on:click="updateCat">미야옹</button>
  <ul class="list text-primary">
    <li>{{ greeting('망고') }}</li>
    <li>{{ cats }}😺</li>
    <li>{{ cats }}😺</li>
    <li>{{ cats }}😺</li>
  </ul>
</template>

<script>
/* import 자리는 위에 */
import { ref, reactive } from "vue";
//ref 원시타입 참조하는 것 reactive 비원시타입 참조

/* 여기는 컴포넌트에 return되는 부분 */
export default {
  //자바스크립트를 쓰려면 setup 함수안에 작성
  setup() {
    let name = ref("망고");
    //name에 망고라는 값을 참조하겠다.
    // console.log(name, name.value);
    let cats = reactive({
      name: '마리',
      age: 100,
      weight: 100,
    })
    const greeting = (name) => { return "짱이뻐" + name };
    const updateName = () => {
      name.value = '공주';
      // console.log(name);
    };
    const updateCat = () => {
      console.log(cats);
      cats.name = '나기',
        cats.age = 5,
        cats.weight = 5

    };
    const consoleLog = () => {
      // console.log('클릭')
    };

    return { name, greeting, consoleLog, updateName, cats, updateCat };
  }
}
</script>

<style>
.app {
  color: red;

}

.list {
  color: green;
  font-size: 2em;
}
</style>
