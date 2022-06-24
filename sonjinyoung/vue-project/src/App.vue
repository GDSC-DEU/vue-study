<!-- html 코드 영역(template) -->
<template>
  <!-- v-bind:class(:class) (데이터 바인딩) -->
  <div 
    :class="nameClass"
  >
    <!-- setup()에 있는 함수 직접 호출 시 다음과 같이 {{greeting(name)}} 으로 표현도 가능 -->
    <!-- setup()에 있는 변수 바인딩 -->
    {{name}}
  </div>

  <!-- v-bind:type(:type), v-bind:value(:value) (데이터 바인딩) -->
  <input 
    :type="type"
    :value="name"
  >

  <!-- click Event(@click)를 위한 button -->
  <button
    class="btn btn-primary"
    @click="updateName"    
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
    // class binding 위한 변수
    const nameClass = ref("name");
    // type binding 위한 변수
    const type = ref("number");

    // name값 변경 위한 함수 
    const updateName = () => {
      // ref 사용시 변수.value로 변경
      name.value = "jinyoung2";
      type.value = "text";
    }
  return {
    name,
    updateName,
    nameClass,
    type
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
