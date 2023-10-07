<script setup>
import HelloWorld from "./components/HelloWorld.vue";
</script>

<template>
  <div class="wrapper">
    <HelloWorld />
  </div>
</template>

<style scoped>
@media only screen and (min-width: 600px) {
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}

.wrapper {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(120, 60, 152, 0.8);
}
</style>
