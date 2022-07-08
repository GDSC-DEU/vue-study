<template>
<div>
  <nav class="navbar navbar-expand-lg bg-light">
    <div class="container-fluid">
      <router-link class="navbar-brand" :to="{ name: 'Home'}">JUYOUNG</router-link>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" :to="{ name: 'Todos' }">Todos</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <div class="container">
    <router-view/>
  </div>
  <Toast 
    v-if="showToast" 
    :message="toastMessage"
    :type="toastAlertType"
  />
</div>
</template>

<script>
import Toast from '@/components/Toast';
import { useToast } from '@/composables/toast';
export default {
  components: {
    Toast
  },
  setup() {
    const {
      toastMessage,
      toastAlertType,
      showToast,
      triggerToast
    } = useToast();

    console.log(showToast.value);

    return {
      toastMessage,
      toastAlertType,
      showToast,
      triggerToast
    }
  }
}
</script>

<style>

</style>