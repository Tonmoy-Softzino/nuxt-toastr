<script setup>

</script>

<template>
  <div class="container">
      <NuxtPage />
  </div>
</template>
<style scoped>
  @use "~/assets/scss/main.scss";

  .container{
    display: flex;
    width: 100vw;
    height: 100vh;
    align-items: center;
    justify-content: center;
  }

  .toastr{
    background: #666666;
  }
</style>
