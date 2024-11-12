<template>
  <div class="remote-component1">
    我是生产者1号组件
    <span>这里是1号span</span>
  </div>
</template>

<script setup>
const remoteComponent1Msg = '生产者1号组件的日志'
console.warn(remoteComponent1Msg)
</script>

<style scoped>
.remote-component1 {
  background: red;
  text-align: center;
}
span {
  background: coral;
}
</style>
