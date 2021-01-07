# Vue与axios整合使用

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

### 请求方法的别名：

- axios.request(config)
- axios.get(url[, config])
- axios.delete(url[, config])
- axios.head(url[, config])
- axios.options(url[, config])
- axios.post(url[, data[, config]])
- axios.put(url[, data[, config]])
- axios.patch(url[, data[, config]])



其中response.data会将json数据直接转为对象

```vue
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<div id="app">
    {{card.address.city}}
</div>
<script type="text/javascript">
    var app = new Vue({
        el : '#app',
        data(){
            return{
                card : ''
            }
        },
        mounted(){
            axios.get('data.json').then(response=>(this.card = response.data))
        },
    })
</script>
```

