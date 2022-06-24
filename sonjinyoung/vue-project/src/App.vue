<!-- html 코드 영역(template) -->
<template>
  <!-- 데이터 바인딩 = {{}} 사용 -->
  <div 
    class="name"
  >
    <!-- setup()에 있는 함수 직접 호출 시 다음과 같이 {{greeting(name)}} 으로 표현도 가능 -->
    <!-- setup()에 있는 함수 호출 2 -->
    {{name}}
  </div>

  <!-- click Event(v-on:click)를 위한 button(하지만 console에만 적용 되고 실제로 div에 있는 text는 변하지 x) -->
  <button
    class="btn btn-primary"
    v-on:click="updateName"    
  >
  click
  </button>
</template>

<!-- JavaScript 코드 영역(script) -->
<script>
export default {
  // Vue3 = composition API -> setup() 사용
  setup() {
    let name = "jinyoung";
    // 함수 호출
    // const greeting = (name) => {
    //  return "Hello " + name;
    // }
    // 함수를 변수로 선언
    // const greet = greeting(name);

    // name값 변경 위한 함수 
    const updateName = () => {
      name = "jinyoung good"
      console.log(name);
    }
  return {
    name,
    updateName
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
