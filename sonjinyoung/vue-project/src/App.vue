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
<!-- Pagination -->
<nav aria-label="Page navigation example" style="display:flex; justify-content:center; margin-top: 50px;">
  <ul class="pagination">
    <!-- 현재 페이지가 1페이지가 아닐 때만 보여줌 -->
    <li v-if="currentPage !==1 " class="page-item">
      <a class="page-link" href="#">Previous</a>
    </li>
    <!-- v-for를 통해 전체 페이지 수에 따라 Pagination을 구현 -->
    <li
      v-for="page in numberOfPages"
      :key=page
      :class = "currentPage === page ? 'active' : ''"
      class="page-item"
    >
      <!-- page가 index이기 때문에 {{page}}를 해야 pagination이 가능 -->
      <a class="page-link" href="#">{{page}}</a>
    </li>
    <!-- 현재 페이지가 마지막 페이지가 아닐 때만 보여줌 -->
    <li v-if="currentPage !== numberOfPages" class="page-item">
      <a class="page-link" href="#">Next</a>
    </li>
  </ul>
</nav>
<!-- end Pagination -->
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

      // 각각 총 데이터 수, 1페이지, 페이지 당 최대 데이터 수
      const totalTodo = ref(0);
      const currentPage = ref(1);
      const limit = 5;

      // Pagination을 위한 computed한 값 
      const numberOfPages = computed(() => {
        return Math.ceil(totalTodo.value/limit);
      });

      // DB에서 데이터를 가져와 새로고침시 데이터가 사라지는 것을 방지
      const getTodos = async () => {
        try {
          // get 요청을 통해 DB에 있는 데이터를 가져와서 변수 res에 저장
          // 페이지네이션을 위해 데이터의 각 페이지당 최대 데이터 수를 5개로 지정
          const res = await axios.get(`http://localhost:3000/todos?_page=${currentPage.value}&_limit=${limit}`);
          // x-total-count = 총 데이터의 개수 => totalPage에 저장
          totalTodo.value = res.headers['x-total-count'];
          // todos의 값을 res의 데이터로 받음
          todos.value = res.data;
        } catch(err) {
          console.log(err);
          error.value = 'Something went wrong';
        }
      };

      // 반드시 getTodos() 호출해주기!
      getTodos();

      // 자식 컴포넌트에서 받은 데이터를 todos 배열에 저장
      // async - await로 비동기 함수 형태로 만듬
      const addToDo = async (todo) => {
        error.value = '';
        // axios.post를 통해 데이터 전송 요청하여 DB에 데이터 저장
        try {
          const res = await axios.post('http://localhost:3000/todos', {
            subject: todo.subject,
            completed: todo.completed,
          });
          // 요청이 성공되어 DB에 저장될 경우 todos에 데이터 추가  
          todos.value.push(res.data);
        } catch (err) {
          console.log(err);
          error.value = 'Something went wrong';
        }
      }

      // delete 버튼 누를 시 todos 안 해당되는 index를 삭제
      // 이때 index 값은 ToDoList.vue의 deleteToDo()를 통해 받음
      const deleteToDo = async (index) => {
        // error message 초기화
        error.value = '';
        // 인자로 받은 index를 통해 지우려고 하는 index의 id값을 변수로 저장
        const id = todos.value[index].id;
        try {
          // delete 요청을 보낼 때 변수로 저장해놓은 id값을 통해 db에 있는 id에 해당되는 데이터를 삭제
          await axios.delete('http://localhost:3000/todos/' + id);
          // DB에서 삭제될 경우 todos의 데이터도 삭제
          todos.value.splice(index, 1);
        } catch(err) {
          console.log(err);
          error.value = 'Something went wrong';          
        }
      }
      
      // ToDoList.vue에서 index 데이터를 받아 completed의 값을 변경
      const toggleToDo = async (index) => {
        // error message 초기화
        error.value = '';
        // 인자로 받은 index를 통해 지우려고 하는 index의 id값을 변수로 저장
        const id = todos.value[index].id;
        try {
          // patch 요청을 통해 DB에 있는 특정 요소를 변경
          await axios.patch('http://localhost:3000/todos/' + id, {
            completed : !todos.value[index].completed
          });
          // DB에서 성공적으로 변경 될 경우 todos 배열에서도 업데이트
          todos.value[index].completed = !todos.value[index].completed; 
        } catch(err) {
          console.log(err);
          error.value = 'Something went wrong';     
        }       
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
      error,
      getTodos,
      totalTodo,
      currentPage,
      numberOfPages
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
