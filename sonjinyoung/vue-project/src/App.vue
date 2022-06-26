<!-- html 코드 영역(template) -->
<template>

<!-- To-do list add form -->
<div class="container mt-4">

<!-- title -->
<h2>To-Do List</h2>

<!-- SearchBar -->
<input
  class="form-control"
  type="text"
  v-model="searchText"
  placeholder="Search To-Do"
>
<!-- end SearchBar -->
<hr/>
<!-- ToDoSimpleForm.vue-->
<!-- @addToDo = context.emit() 첫번째 인자에 있는 이벤트 이름 -->
<ToDoSimpleForm @add-ToDo="addToDo" />
<!-- end ToDoSimpleForm.vue-->

<!-- DB에 저장되지 않았을 경우 error message -->
<div style="color:red;">
  {{error}}
</div>

<!-- empty todos -->
<div
  class="mt-2"
  v-show="!filteredTodos.length"
  style="color:red;"
>
  There is nothing to display
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
<ToDoList 
  :todos = "filteredTodos"
  @toggle-ToDo="toggleToDo"
  @delete-ToDo="deleteToDo"
/>
<!-- end ToDoCard -->

</div>
<!-- end To-do list add form -->

</template>

<!-- JavaScript 코드 영역(script) -->
<script>
  // reactive state를 위한 ref import
  // 검색 결과 감지를 위한 computed import
  import {ref, computed} from 'vue';

  // axios import (DB에 http request 요청 위한 라이브러리)
  import axios from 'axios';

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

      // searchBar의 검색 결과 text
      const searchText = ref('');

      // 데이터 전송 실패했을 때를 위한 변수 error
      const error = ref('');
  
      // 자식 컴포넌트에서 받은 데이터를 todos 배열에 저장
      const addToDo = (todo) => {
        error.value = '';
        // axios.post를 통해 데이터 전송 요청하여 DB에 데이터 저장
        axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed,
        // 요청이 성공되어 DB에 저장될 경우 todos에 데이터 추가  
        }).then(res => {
          console.log(res);
          todos.value.push(res.data);
        // 요청이 실패되어 DB에 저장되지 않을 경우 error 메시지 출력  
        }).catch(err => {
          console.log(err);
          error.value = 'Something went wrong';
        });
      };

      // delete 버튼 누를 시 todos 안 해당되는 index를 삭제
      // 이때 index 값은 ToDoList.vue의 deleteToDo()를 통해 받음
      const deleteToDo = (index) => {
        todos.value.splice(index, 1);
      }
      
      // ToDoList.vue에서 index 데이터를 받아 completed의 값을 변경
      const toggleToDo = (index) => {
        todos.value[index].completed = !todos.value[index].completed; 
      }

      // searchBar에서 검색한 결과를 나타내는 computed한 값
      const filteredTodos = computed(() => {
        // 만약 사용자가 검색을 했다면 원래 todoList에서 filter한 결과를 가져옴
        if(searchText.value) {
          // filter()는 todos 배열에 있는 각 todo들에 대해 todo의 subject값이 searchText 값과 일치하면 필터링
          return todos.value.filter(todo => {
            return todo.subject.includes(searchText.value);
          })
        }
        // 만약 사용자가 검색하지 않았다면 원래 todoList 결과를 가져옴
        return todos.value
      });

    return {
      todos,
      addToDo,
      deleteToDo,
      toggleToDo,
      searchText,
      filteredTodos,
      error
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
