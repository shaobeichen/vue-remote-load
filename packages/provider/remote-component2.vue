<template>
  <div class="remote-component2">我是生产者2号组件</div>
</template>

<script setup>
const remoteComponent2Msg = '生产者2号组件的日志'
console.warn(remoteComponent2Msg)
</script>

<style scoped>
.remote-component2 {
  background: green;
  text-align: center;
}
</style>
