<!-- html 코드 영역(template) -->
<template>
  <!-- v-bind:class(:class) (데이터 바인딩) -->
  <div 
    class="name"
  >
    <!-- setup()에 있는 함수 직접 호출 시 다음과 같이 {{greeting(name)}} 으로 표현도 가능 -->
    <!-- setup()에 있는 변수 바인딩 -->
    {{name}}
  </div>

  <!-- v-model을 통해 양방향 바인딩 구현 -->
  <!-- 양방향 바인딩을 통해 input에 쳐놓은 값을 js 변수에도 저장 -->
  <input 
    type="text"
    v-model="name"
  >

  <!-- click Event(@click)를 위한 button -->
  <button
    class="btn btn-primary"
    @click="onSubmit"    
  >
  click
  </button>
</template>

<!-- JavaScript 코드 영역(script) -->
<script>
  // reactive state를 위한 ref import
  import {ref} from 'vue';
export default {
  // Vue3 = composition API -> setup() 사용
  setup() {
    // ref 사용 시 ref()안에 기본 자료형 넣기
    const name = ref("jinyoung");
 
    // name값 변경 위한 함수 
    const onSubmit = () => {
      // ref 사용시 변수.value로 변경
      return name.value 
    }
  return {
    name,
    onSubmit
  };
}
}
</script>

<!-- CSS 코드 영역(style) -->
<style>
.name {
  color:red;
}
</style>
