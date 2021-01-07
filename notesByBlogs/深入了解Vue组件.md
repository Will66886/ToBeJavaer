# 深入了解Vue组件

## 1、组件注册

### 1.1、组件名

组件名是Vue.component的第一个参数

组件可以是短横线分隔命名和驼峰命名

```js
Vue.component('my-component-name', { /* ... */ })
Vue.component('MyComponentName', { /* ... */ })
```

强烈建议使用短横线命名

### 1.2、全局注册



