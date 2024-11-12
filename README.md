# vue-remote-load

Vue load remote component. 如何加载 Vue 远程组件。

![preview](./assets/image.png)

上图为演示效果及解释。

```js
// 通过配置指定的数据结构
// 动态的渲染本地和远程组件
const components = [
  {
    name: 'remote-component1',
    remote: true,
  },
  {
    name: 'local-button',
    remote: false,
  },
  {
    name: 'remote-component2',
    remote: true,
  },
]
```

# 生产者使用 (远程组件方)

新建一个命令行窗口执行

```
pnpm dev:p
```

# 消费者使用 (容器方)

新建一个命令行窗口执行

```
pnpm install
pnpm dev:c
```

# 预览

浏览器打开 http://localhost:3000
