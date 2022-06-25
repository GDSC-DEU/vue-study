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
export default {

}
</script>

<style>

</style>