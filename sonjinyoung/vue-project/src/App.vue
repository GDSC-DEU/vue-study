<!-- html 코드 영역(template) -->
<template>

<!-- To-do list add form -->
<div class="container mt-2">

<!-- title -->
<h2>To-Do List</h2>

<!-- ToDoSimpleForm.vue-->
<!-- @addToDo = context.emit() 첫번째 인자에 있는 이벤트 이름 -->
<ToDoSimpleForm @add-ToDo="addToDo" />
<!-- end ToDoSimpleForm.vue-->

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

  // ToDoSimpleForm.vue import
  import ToDoSimpleForm from './components/ToDoSimpleForm.vue';

  export default {
    // component import시 반드시 export default -> components에 추가
    components : {
      ToDoSimpleForm
    },

    // Vue3 = composition API -> setup() 사용
    setup() {
      // todos 배열에 초깃값 설정
      const todos = ref([
      ]);
  
      // 자식 컴포넌트에서 받은 데이터를 todos 배열에 저장
      const addToDo = (todo) => {
        todos.value.push(todo);
      };

      // delete 버튼 누를 시 todos 안 해당되는 index를 삭제
      const deleteTodo = (index) => {
        todos.value.splice(index, 1);
      }

    return {
      todos,
      addToDo,
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
