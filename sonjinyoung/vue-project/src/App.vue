<!-- html 코드 영역(template) -->
<template>
<!-- To-do list add form -->
<div class="container mt-2">
<!-- title -->
<h2>To-Do List</h2>
<!-- button에서 submit버튼 누를 시 @submit을 통해 onSubmit 함수 실행 -->
<form
  @submit.prevent="onSubmit"
  class="d-flex"
>
  <!-- input -->  
  <div class="flex-grow-1 mr-2">
  <!-- v-model을 통해 양방향 바인딩 구현! -->
  <!-- 양방향 바인딩을 통해 input에 입력한 값을 js 변수에도 저장 -->
    <input
      class="form-control" 
      type="text"
      v-model="todo"
      placeholder="please add To-do"
    >
  </div>
  <!-- button -->  
  <div>
    <button
      style="margin-left:7px;"
      class="btn btn-primary"
      type="submit" 
    >
      add
    </button>
  </div>
</form>
<!-- add card -->
<!-- v-for를 통해 todos에 있는 객체가 추가 될때마다 card로 생성(v-for와 :key는 한 세트) -->
<div 
  class="card mt-2"
  v-for="todo in todos"
  :key="todo.id"
>
  <div class="card-body p-2">
    <!-- 설정한 todos 바인딩 -->
    {{todo.subject}}
  </div>
</div>
</div>

</template>

<!-- JavaScript 코드 영역(script) -->
<script>
  // reactive state를 위한 ref import
  import {ref} from 'vue';
export default {
  // Vue3 = composition API -> setup() 사용
  setup() {
    // ref 사용 시 ref()안에 기본 자료형 or 참조형(reactive도 가능) 넣기
    const todo = ref("");
    // todos 배열에 초깃값 설정
    const todos = ref([
    ]);
 
    // input에서 입력 한 값을 todos 배열에 저장
    const onSubmit = () => {
      // ref 사용시 변수.value로 변경
      return todos.value.push({
        id:Date.now(),
        subject:todo.value
      }); 
    }
    return {
      todo,
      todos,
      onSubmit
    };
  } 
}
</script>

<!-- CSS 코드 영역(style) -->
<style>

</style>
