<!-- html 코드 영역(template) -->
<template>

<!-- To-do list add form -->
<div class="container mt-2">

<!-- title -->
<h2>To-Do List</h2>

<!-- form -->
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
      placeholder="Add To-Do-List :)"
    >
  </div>
  <!-- input end -->
    
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
  <!-- button end -->  

</form>
<!-- form end -->

<!-- empty todos -->
<div
  class="mt-2"
  v-show="!todos.length"
  style="color:red;"
>
  Please Add To-do
</div>

<!-- Error Message -->  
<div
  class="mt-2"
  style="color:red;"
  v-show="hasError"
>
    This field cannot be empty
</div>

<!-- add card -->
<!-- v-for를 통해 todos에 있는 객체가 추가 될때마다 card로 생성(v-for와 :key는 한 세트) -->
<div 
  class="card mt-2"
  v-for="(todo,index) in todos"
  :key="todo.id"
>
  <div class="card-body p-2 d-flex align-items-center">
    <!-- check form -->
    <div class="form-check flex-grow-1">
      <!-- check box(v-model로 true, false 값에 따라 checking) -->
      <input
        class="form-check-input"
        type="checkbox"
        v-model="todo.completed"
      >
      <!-- end check box -->

      <!-- todo value -->
      <!-- :class를 통해 checking될 경우 스타일을 설정 -->
      <label 
        class="form-check-label"
        :class="{ todoCheck : todo.completed }"
      >
        <!-- 설정한 todos 바인딩 -->
        {{todo.subject}}
      </label>
      <!-- end todo value -->
    </div>
    <!-- end check form -->

    <!-- delete Button -->
    <div>
        <button 
          class="btn btn-danger btn-sm"
          @click="deleteTodo(index)"
        > 
          delete 
        </button>
    </div>
    <!-- end delete Button -->

  </div>
</div>
<!-- end card -->

</div>
<!-- end To-do list add form -->

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
    // Error Message의 조건부 바인딩을 위한 변수
    const hasError = ref(false);
 
    // input에서 입력 한 값을 todos 배열에 저장
    const onSubmit = () => {
      // add 버튼을 누를 시 emptyString일 경우
      // hasError의 값이 true로 바뀌어 조건부 바인딩에 의해 ErrorMessage 출력 
      if(todo.value === '') {
        hasError.value = true;
      } else {
      // emptyString이 아니면 데이터가 todos 배열에 추가되고 hasError의 값이 false로 변환
        todos.value.push({
          id:Date.now(),
          subject:todo.value,
          completed:false,
        });
        hasError.value = false;
      }
    };

    // delete 버튼 누를 시 todos 안 해당되는 index를 삭제
    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      todo,
      todos,
      onSubmit,
      hasError,
      deleteTodo
    };
  } 
}
</script>

<!-- CSS 코드 영역(style) -->
<style>
  .todoCheck {
    text-decoration: line-through;
    color:gray;
  }
</style>
