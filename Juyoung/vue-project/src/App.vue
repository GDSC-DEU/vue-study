<template>
<div>
  <Navbar />
  <div class="container">
    <router-view/>
  </div>
  <Toast />
</div>
</template>

<script>
import Toast from '@/components/Toast';
import Navbar from '@/components/Navbar'
export default {
  components: {
    Toast,
    Navbar
  },
}
</script>

<style scoped>

</style>