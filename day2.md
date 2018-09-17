# Vue.js - Day2



## 今天主要学习目标
1. 能够定义和使用 `过滤器`
2. 了解实例的 `生命周期` 和 `生命周期函数`
3. 能够使用 `axios` 发起 Ajax 请求
   + ES6 中的 Promise
   + ES7 中的 async 和 await
4. 带有数据交互的品牌列表案例
5. 能够使用 Vue 中常见的 `过渡动画`



## 1. 过滤器
+ `"2018-01-25T02:10:02.945Z"`    =>   `2018-01-25 `
+ 概念：过滤器本质上就是一个函数，**可被用作一些常见的文本格式化**。
+ 过滤器只可以用在两个地方：**mustache 插值表达式和 v-bind 表达式**。
+ 过滤器应该被添加在 JavaScript 表达式的尾部，由**“管道”符**指示；

### 1.1 全局过滤器
1. **使用全局过滤器的语法**
   + `<span>{{ dt | 过滤器的名称 }}</span>`
2. **定义全局过滤器的语法**
   - `Vue.filter('过滤器的名称', function(originVal){ /* 对数据进行处理的过程，在这个 function 中，最后必须 return 一个处理的结果 */ })`
3. **使用过滤器的注意事项：**
   + 如果想拿管道符前面的值，通过 function 的第一个形参来拿
   + 过滤器中，一定要返回一个处理的结果，否则就是一个无效的过滤器
   + 在调用过滤器的时候，直接通过 () 调用就能传参； 从过滤器处理函数的第二个形参开始接收传递过来的参数
   + 可以 多次 使用 | 管道符 一次调用多个过滤器

### 1.2 私有过滤器
+ 语法：
```js
var vm = new Vue({
  el: '#app',
  data: {},
  filters: {
  	// 这是 古老的 ES3写法
    过滤器的名称: function(originVal){},
    // 这是最新的ES6写法【推荐】
    过滤器的名称(originVal){
        return 处理的结果
    }
  }
})
```

> 注意过滤器查找顺序问题：就近原则



## 2. `Vue.$mount()` 动态挂载实例【了解】

语法：Vue.$mount("选择器 - 指定要控制的区域")



## 3. `template`属性指定模板

注意：如果 在 `new Vue` 实例的时候，既指定了 `el` 又指定了 `template`，则 会把 `template` 指定的模板结构，替换掉 `el` 的模板结构；



## 4. [vue实例的生命周期](https://cn.vuejs.org/v2/guide/instance.html#实例生命周期)



### 4.1 什么是生命周期（每个实例的一辈子）

**生命周期：**实例的生命周期，就是一个阶段，从创建到运行，再到销毁的阶段；

**生命周期函数：**在实例的生命周期中，在特定阶段执行的一些特定的事件，这些事件，叫做 生命周期函数；

+ 生命周期函数 = 生命周期钩子 = 生命周期事件



### 4.2 主要的生命周期函数分类

 - **创建期间**的生命周期函数：(特点：每个实例一辈子只执行一次)
   + beforeCreate：创建之前，此时 data 和 methods 尚未初始化
   + **created**(第一个重要的函数，此时，data 和 methods 已经创建好了，可以被访问了)
   + beforeMount：挂载模板结构之前，此时，页面还没有被渲染到浏览器中；
   + **mounted**（第二个重要的函数，此时，页面刚被渲染出来；如果要操作DOM元素，最好在这个阶段）
 - **运行期间**的生命周期函数：（特点：按需被调用 至少0次，最多N次）
   + beforeUpdate：数据是最新的，页面是旧的
   + updated：页面和数据都是最新的
 - **销毁期间**的生命周期函数：(特点：每个实例一辈子只执行一次)
   + beforeDestroy：销毁之前，实例还正常可用
   + destroyed：销毁之后，实例已经不工作了



## 5. Promise、async 和 await

### 5.1 什么是Promise

1. **概念：**Promise 是ES6 中的新语法，Promise 是一个构造函数；每个new 出来的 Promise 实例对象，都代表一个异步操作；
2. **作用：**解决了回调地狱的问题；
   + 回调地狱，指的是回调函数中，嵌套回调函数的代码形式；如果嵌套的层级很深，就是回调地狱；
   + 回调地狱，不利于代码的阅读、维护和后期的扩展；

## 5.2 Promise 怎么用

1. **创建形式上的异步操作：**

   ```js
   const p = new Promise()
   ```

2. **创建具体的异步操作：**

   ```js
   const p = new Promise(function(successCb, errorCb){
       // 在这个 function 中定义具体的异步操作
   })
   ```

### 5.2 async 和 await 的作用

ES7 中的 async 和 await 可以简化 Promise 调用，提高 Promise 代码的 阅读性 和 理解性；



## 6. [axios 实现数据接口请求](https://www.npmjs.com/package/axios)
1. **之前的学习中，如何发起数据请求？**
   1. 最开始封装 `XMLHttpRequest` 对象发起Ajax请求
   2. 使用Jquery中提供的工具函数：
      + `$.ajax({配置对象})`
      + `$.post(url地址, function(){})`
      + `$.get(url地址, 处理函数)`
2. **现在用更高端的 axios 来发起Ajax请求：**
   + 只支持 `get` 和 `post` 请求，无法发起 `JSONP` 请求；
   + 如果涉及到 `JSONP` 请求，可以让后端启用 `cors` 跨域资源共享即可；
3. **在Vue中，还可以使用 `vue-resource` 发起数据请求**
   + `vue-resource` 支持 get, post, jsonp请求【但是，Vue官方不推荐使用这个包了！】



### 6.1 axios的使用

1. 用于测试的URL请求资源地址：
   + **get 请求地址：** `http://www.liulongbin.top:3005/api/get`
   + **post请求地址：**`http://www.liulongbin.top:3005/api/post`
2. 使用 `axios.get()` 和 `axios.post()` 发起请求
3. 使用拦截器实现 loading 效果【拓展 - 了解即可】
4. 使用 async 和 await 结合 axios 发起 Ajax 请求



## 7. 优化和完善 品牌列表案例

1. 把请求品牌列表的 Ajax 方法，通过 async 和 await 进行优化；
2. 添加品牌
3. 删除品牌
4. 搜索功能



## 8. [Vue中的动画](https://cn.vuejs.org/v2/guide/transitions.html)

- **为什么要有动画：**动画能够增加页面趣味性，目的是**为了让用户更好的理解页面的功能**；
- **注意：**Vue中的动画，都是简单的过渡动画，并不会有 CSS3 那么炫酷；

### 8.1 Vue中动画的基本介绍

1. 每个都动画分为两部分：
   - **入场动画**：从不可见（flag = false） -> 可见（flag = true）
   - **出场动画**：可见（flag = true） -> 不可见（flag = false）
2. 入场时候，Vue把这个动画，分成了**两个时间点**和**一个时间段**：
   - `v-enter`：入场前的样式
   - `v-enter-to`：入场完成以后的样式
   - `v-enter-active`：入场的时间段
3. 离场时候，Vue把动画，分成了**两个时间点**和**一个时间段**：
   - `v-leave`：离场之前的样式
   - `v-leave-to`：离场完成以后的样式
   - `v-leave-active`：离场的时间段



### 8.2 使用过渡类名

1. 把需要添加动画的元素，使用v-if或v-show进行控制

2. 把需要添加动画的元素，使用Vue提供的元素 `<transition></transition>` 包裹起来

3. 添加两组类：

   ```css
   .v-enter,
   .v-leave-to{
       opacity: 0;
       transform: translateX(100px);
   }
   
   .v-enter-active,
   .v-leave-active{
     	transition: all 0.5s ease;
   }
   ```


### 8.3 [使用第三方 CSS 动画库](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

1. 把需要添加动画的元素，使用v-if或v-show进行控制
2. 把需要添加动画的元素，使用Vue提供的元素 `<transition></transition>` 包裹起来
3. 为  `<transition></transition>`  添加两个属性类`enter-active-class`, `leave-active-class`
4. 把需要添加动画的元素，添加一个 `class="animated"`



### 8.4 [v-for 的列表过渡](https://cn.vuejs.org/v2/guide/transitions.html#%E5%88%97%E8%A1%A8%E8%BF%87%E6%B8%A1)

1. 把v-for循环渲染的元素，添加 `:key` 属性

2. 在 v-for循环渲染的元素外层，包裹 `<transition-group> ` 标签

3. 添加两组类即可：

   ```css
   .v-enter,
   .v-leave-to {
         opacity: 0;
         transform: translateY(100px);
   }
   
   .v-enter-active,
   .v-leave-active {
         transition: all 0.8s ease;
   }
   ```




### 8.5 列表的排序过渡

`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，**还可以改变定位**。要使用这个新功能只需了解新增的 `v-move` 特性，**它会在元素的改变定位的过程中应用**。

- `v-move` 和 `v-leave-active` 结合使用，能够让列表的过渡更加平缓柔和：

```css
/*控制要被删除的元素，脱离标准流*/
.v-leave-active{
  position: absolute;
}

/*控制后续的元素，通过过渡位移到目标位置*/
.v-move{
  transition: all 0.8s ease;
}
```





## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [pagekit/vue-resource](https://github.com/pagekit/vue-resource)
6. [navicat如何导入sql文件和导出sql文件](https://jingyan.baidu.com/article/a65957f4976aad24e67f9b9b.html)
7. [贝塞尔在线生成器](http://cubic-bezier.com/#.4,-0.3,1,.33)