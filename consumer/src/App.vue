<template>
  <div style="background: blue; color: white; padding: 20px">
    <div>我是消费者容器</div>
    <RemoteChild />
  </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent, ref, onMounted } from 'vue'
import * as Vue from 'vue'
import { loadModule } from 'vue3-sfc-loader'

const options = {
  moduleCache: {
    vue: Vue,
  },
  async getFile(url) {
    const res = await fetch(url)
    const code = await res.text()
    return code
  },
  addStyle(textContent) {
    const style = Object.assign(document.createElement('style'), {
      textContent,
    })
    const ref = document.head.getElementsByTagName('style')[0] || null
    document.head.insertBefore(style, ref)
  },
}

const RemoteChild = defineAsyncComponent(async () => {
  const res = await loadModule(
    `http://localhost:8080/remote-component.vue?t=${+new Date()}`,
    options,
  )
  console.log('res', res)
  return res
})
</script>
