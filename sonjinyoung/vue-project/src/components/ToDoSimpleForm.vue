<template>
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
</template>

<script>
  // reactive state를 위한 ref import
  import {ref} from 'vue';
export default {
    emits: ['add-ToDo'],

    // props와 emit을 위해 인자에 props, context 추가
    setup(props, {emit}) {
        // ref 사용 시 ref()안에 기본 자료형 or 참조형(reactive도 가능) 넣기
        const todo = ref("");

        // Error Message의 조건부 바인딩을 위한 변수
        const hasError = ref(false);

        // input에서 입력 한 값을 todos 배열에 저장
        const onSubmit = () => {
            // add 버튼을 누를 시 emptyString일 경우
            // hasError의 값이 true로 바뀌어 조건부 바인딩에 의해 ErrorMessage 출력 
            if(todo.value === '') {
            hasError.value = true;
            } else {
                // context.emit을 통해 자식 컴포넌트 -> 부모 컴포먼트로 데이터 이동
                // emit의 첫번째 인자로는 이벤트 이름, 두번째는 보낼 데이터
                emit('add-ToDo', {
                    id: Date.now(),
                    subject: todo.value,
                    completed: false,
                });
                // hasError의 값이 false로 변환
                hasError.value = false;
                todo.value = '';
            }
        };
        return {
            todo,
            hasError,
            onSubmit,
        }
    }
}
</script>

<style>

</style>