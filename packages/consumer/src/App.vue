<template>
  <div style="background: blue; color: white; padding: 20px; width: 300px; margin: 0 auto">
    <div>
      我是消费者容器
      <span>这里是消费者span</span>
    </div>
    <component v-for="(item, index) in remoteComponent" :key="index" :is="item"></component>
  </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent, ref, markRaw } from 'vue'
import type { Component } from 'vue'
import * as Vue from 'vue'
import { loadModule } from 'vue3-sfc-loader'
import componentConfig from '../../../config'

/**
 * 动态引入组件
 * @param name 组件名
 * @param importUrl 引入所有的组件 import.meta.glob('@/components/config/**')
 * @returns
 */
const batchDynamicComponents = (name: string, importUrl: Record<string, Component>) => {
  const components = importUrl
  const componentMap = Object.assign(
    {},
    ...Object.keys(components).map((item) => {
      const name = item?.split('/')?.pop()?.replace('.vue', '') || ''
      return {
        [name]: components[item],
      }
    }),
  )
  const importComponent = componentMap[name]

  if (!importComponent) return ''

  return markRaw(defineAsyncComponent(importComponent))
}

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

const remoteComponent = ref([])
for (const item of componentConfig) {
  if (!item.remote) {
    const localChild = batchDynamicComponents(item.name, import.meta.glob('@/components/**/*.vue'))
    remoteComponent.value.push(localChild)
    continue
  }
  const remoteChild = defineAsyncComponent(async () => {
    const res = await loadModule(
      `http://localhost:8080/${item.name}.vue?t=${+new Date()}`,
      options as any,
    )
    console.log(`组件返回 ${item.name}`, res)
    return res
  })
  remoteComponent.value.push(markRaw(remoteChild))
}
</script>
