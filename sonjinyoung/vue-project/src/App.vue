<!-- html 코드 영역(template) -->
<template>
  <!-- 데이터 바인딩 = {{}} 사용 -->
  <div class="name">{{name}}</div>
</template>

<!-- JavaScript 코드 영역(script) -->
<script>
export default {
  // Vue3 = composition API -> setup() 사용
  setup() {
    const name = "jinyoung";
  
  return {
    name
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
