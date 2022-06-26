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
      <!-- check box(:value와 @change로 자식 -> 부모 컴포넌트에 index값 보냄(props 주의 사항)) -->
      <input
        class="form-check-input"
        type="checkbox"
        :checked="todo.completed"
        @change="toggleToDo(index)"
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
          @click="deleteToDo(index)"
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
    },
    emits: ['toggle-ToDo', 'delete-ToDo'],
    setup(props, {emit}) {
        // 부모 컴포넌트에 todo index값을 보내기 위한 함수
        const toggleToDo = (index) => {
            emit('toggle-ToDo', index);
        };

        // index 값을 부모 컴포넌트에 보내줌
        const deleteToDo = (index) => {
            emit('delete-ToDo', index);
        }

        return {
            toggleToDo,
            deleteToDo,
        }
    }
}
</script>

<style>

</style>