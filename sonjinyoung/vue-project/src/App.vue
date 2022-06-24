<!-- html 코드 영역(template) -->
<template>
  <!-- 데이터 바인딩 = {{}} 사용 -->
  <div class="name">
    <!-- setup()에 있는 함수 직접 호출 시 다음과 같이 {{greeting(name)}} 으로 표현도 가능 -->
    <!-- setup()에 있는 함수 호출 2 -->
    {{greet}}
  </div>
</template>

<!-- JavaScript 코드 영역(script) -->
<script>
export default {
  // Vue3 = composition API -> setup() 사용
  setup() {
    const name = "jinyoung";
    const greeting = (name) => {
      return "Hello " + name;
    }
    const greet = greeting(name);
  return {
    name,
    greeting,
    greet
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
