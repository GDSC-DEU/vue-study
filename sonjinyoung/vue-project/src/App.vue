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

<!-- ToDoCard -->
<!-- :todos = props(부모 컴포넌트 -> 자식 컴포넌트) -->
<ToDoList :todos = "todos" @toggle-ToDo="toggleToDo" />
<!-- end ToDoCard -->

</div>
<!-- end To-do list add form -->

</template>

<!-- JavaScript 코드 영역(script) -->
<script>
  // reactive state를 위한 ref import
  import {ref} from 'vue';

  // ToDoSimpleForm.vue import
  import ToDoSimpleForm from './components/ToDoSimpleForm.vue';

  // ToDoList.vue import
  import ToDoList from './components/ToDoList.vue';

  export default {
    // component import시 반드시 export default -> components에 추가
    components : {
      ToDoSimpleForm,
      ToDoList
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
      
      // ToDoList.vue에서 index 데이터를 받아 completed의 값을 변경
      const toggleToDo = (index) => {
        todos.value[index].completed = !todos.value[index].completed; 
      }

    return {
      todos,
      addToDo,
      deleteTodo,
      toggleToDo
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
