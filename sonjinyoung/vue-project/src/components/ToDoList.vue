<template>
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
</template>

<script>
export default {
    // App.vue에서 받는 데이터를 props를 통해 설정
    props: {
        todos : {
            type: Array,
            required : true
        }
    }
}
</script>

<style>

</style>