# Vue.js 基础 - Day1



## 0. 课程安排

1. Vue基础语法（天数会少一些  大概5天左右）
   + Vue的指令
   + Vue的动画
   + Vue的组件
   + Vue路由
   + Vue中组件生命周期
   + Vue中使用 axios 发起Ajax数据请求
   + 穿插讲解 ES6 、 ES7 新语法   let const   箭头函数    展开运算符  解构赋值  数组的新方法（forEach， map，filter, findIndex）  (async  await) ES7 的语法   Promise  定义属性和方法的新方式
   + Webpack（构建工具）
2. Vue项目（课程天数会多一些   大概是7天左右）



## 1. 今天的主要内容

1. 主要了解什么是Vue
2. Vue中基本的代码结构
3. 学习基本的Vue指令 `{{  }}`
4. 学习Vue前的技术准备：
   + 掌握 HTML + CSS + JS 基本网页制作能力
   + 了解Node基础概念、包、模块化，会用 npm 维护项目中的包即可
   + ES6 基础语法要会用，在Vue课程中我们也会补充更多的ES6



## 2. 框架和库的区别

+ **概念：**小而巧的是库； 大而全的是框架
 + **框架：**是一套完整的解决方案；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。


 + **库（插件）：**提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。



## 3. 为什么要学习流行框架

+ 企业角度：
  - 企业中需要使用流行框架来提高项目的开发效率；（本质目的：节约成本）
+ 个人角度：
  + 有需求就有买卖
  + 为了能有一个比较不错的收入
  + 提高竞争力和对技术的把握程度
    + 人无我有，人有我精
  + 大家学习的不仅仅是框架
    + 接触到最新的开发模式
    + 最新的开发思想(比如MVVM的开发思想)
    + 最前沿的技术 ES6 ，ES7



## 4. 什么是Vue.js

+ Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！
  + Angular.js  出现**最早**的前端框架，曾经很火，但是现在已经 被边缘化了；也不好学；
    + Angular 1.x     学起来好麻烦；
    + Angular 2.x ~ 5.x    学习起来相对简单；
  + React.js   是目前**最流行**的一个框架；是用的人最多的一个框架；但是，学习起来也比较难；因为在 React中，所有的功能，都要用 Javascript 来实现；
  + Vue.js 是目前**最火**的前端框架；Vue 学习起来非常容易； Vue 是中国人 `尤雨溪 ` 开发的； 文档非常友好；
+ Vue.js 是一套构建用户界面的框架，**只关注视图（页面）层的开发**，易于上手；
+ 前端的主要工作是什么？
  + 实现基本的页面结构
  + 实现炫酷的页面效果
  + 实现数据交互
+ 前端的发展历程
  + 在Web端开发中，刚开始没有前端岗位，那时候只有美工（作图、负责画页面结构 dreamware）
  + 前端就是一些活儿不好的美工，另辟蹊径，打通了前端的岗位；
  + HTML +  Table 布局 + CSS2
  + CSS + Div 布局
  + 使用原生的 Javascript 写页面的行为 （兼容性）
  + Jquery + 模板引擎 + Ajax（老年程序员）
  + 前端流行框架诞生（要解决的问题：就是 避免程序员手动操作DOM元素）



## 5. Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别（了解内容）

 + MVC 主要是后端的分层开发思想；把 一个完整的后端项目，分成了三个部分：
    + Model：（数据层）主要负责 数据库的操作；
    + View：（视图层）所有前端页面，统称为 View 层
    + Controller：（业务逻辑层）主要处理对应的业务逻辑；（对于后台来说，这是开发的重点）
 + MVVM是**前端页面的分层开发思想**，主要关注于 **视图层** 分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View,  ViewModel
    + Model 是 页面中，需要用到的数据
    + View 是页面中的HTML结构；
    + ViewModel 是 一个 中间的调度者,提供了双向数据绑定的概念；
 + 为什么有了MVC还要有MVVM
     + 因为 MVC是后端的开发思想，并没有明确定义前端的页面该如何开发；
     + MVVM 是前端的页面的开发思想，把每个页面，分成了三个部分，同时 VM 作为 MVVM 的核心，提供了双向数据绑定的概念，前端程序员，不需要手动渲染页面了，而且，页面数据发送变化，也不需要程序员手动把 数据的变化同步到Model中；这所有的操作，都是 VM 自动完成的！
     + 有了 MVVM 的思想以后，前端只关心 页面交互逻辑，不关心页面如何渲染；



## 6. Vue.js 基本代码 和 MVVM 之间的对应关系

1. 注意：Vue中，不推荐程序员手动操作DOM元素；所以，在Vue项目中，没有极其变态的需求，一般不要引入 Jquery；
2. Vue代码解析执行的步骤：
   1. 当 VM 实例对象，被 创建完成之后，会立即解析 el 指定区域中的所有代码；
   2. 当 VM 在解析 el 区域中所有代码的时候，会把 data 中的数据，按需，填充到 页面指定的区域；
3. 注意：每当 vm 实例对象，监听到 data 中数据发生了变化，就会立即 重新解析 执行 el 区域内，所有的代码；



## 7. Vue调试工具`vue-devtools`的安装和使用

[Vue.js devtools - 翻墙安装方式 - 推荐](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)



## 8. 什么是Vue中的指令

定义： Vue 中，通过一些特殊的语法，扩展了 HTML 的能力；

+ 将来 创建 Vue 实例的时候，Vue 会把 这些指令 都进行解析，从而，根据不同的指令，执行不同的操作、渲染不同的结果；



### 8.1 Vue指令之 `插值表达式 {{ }}`

1. 基本使用演示
   在指定的位置动态插入内容，例如：
   ```html
   <p>{{msg}}</p>
   ```
2. 在插值表达式中 使用简单的语句
3. 注意：插值表达式只能用在元素的内容区域；不能用在元素的属性节点中；



### 8.2 Vue指令之 `v-cloak`

1. 解决的问题
    + 插值表达式有闪烁的问题（`v-cloak` 指令来解决闪烁问题）
2. 应用场景
    + 当 网络比较卡的时候，我们可以为 最外层的 元素，添加 `v-cloak` ，防止用户看到 插值表达式



### 8.3 Vue指令之 `v-text`

1. 基本使用
   在 元素的属性节点上，添加`v-text`指令，例如：
   ```html
   <p v-text="msg"></p>
   ```
2. 在`v-text`中 使用简单的语句
3. `v-text` 与 `{{}}` 的区别
   + 是否覆盖内容
   + 指令闪烁问题
4. 应用场景
    + 向指定元素的内容区域中，渲染指定的文本；



### 8.4 Vue指令之 `v-html`

1. 基本使用
    在 元素的属性节点上，添加`v-html`指令，例如：
    ```html
    <div v-html="msg"></div>
    ```
2. 应用场景
   当 服务器返回的数据中，包含的HTML的标签，此时，这些标签只能使用 `v-html` 来渲染；



### 8.5 Vue指令之 `v-bind:` 属性绑定

1. 基本使用
   + `v-bind:` 是为 html 属性节点动态绑定数据的，例如：
      ```html
      <button v-bind:title="titleStr">按钮</button>
      ```
2. 应用场景
   + 如果元素的属性值，需要动态地进行绑定，则需要使用`v-bind:` 指令
3. 简写形式
   + `v-bind:` 指令可以简写成 `:`，例如，可以简写成如下格式：
      ```html
      <button :title="titleStr">按钮</button>
      ```



### 8.6 Vue指令之 `v-on:` 事件绑定

1. 基本使用：
`v-on:` 的作用，是为 HTML 元素，绑定 事件处理函数，例如：
    ```html
    <input type="button" value="按钮" v-on:click="事件处理函数名" />
    ```
2. 绑定事件处理函数并传参：
    ```html
    <input type="button" value="按钮" v-on:click="show(123)" />
    ```
3. 简写形式：
`v-on:` 指令可以简写成 `@`，例如，可以简写成如下格式：
   ```html
   <input type="button" value="按钮" @click="事件处理函数名" />
   ```



### 8.7 【案例】跑马灯效果



### 8.8 Vue指令之 `v-model` 实现 双向数据绑定

1. 基本使用：
  + 可以把页面上数据的变化，自动同步更新到 `VM` 实例的 `data` 中。例如：
    ```html
    <input type="text" v-model="msg"/>
    ```
2. 和 `v-bind:`的区别：
   + `v-bind:` 只能实现单向的数据同步 `data ---> 页面`；
   + `v-model` 可以实现双向的数据同步 `data <--> 页面`；
3. 注意： 
   + `v-model` 只能 和 **表单元素** 配合使用，例如 `input、select、textarea` 等；
   + `v-model` 是 Vue 中 **唯一支持** 双向数据绑定的指令；
3. 应用场景：
   + 【案例】自动计算文本框中，字符串的长度
   + 【案例】简易加法计算器



### 8.9 在Vue中使用class样式

1. 类名数组：
   + 通过 `v-bind:` 为元素的 `class` 绑定具体的类名：
    ```html
    <p :class="['thin', 'red', 'big']">彬哥最帅</p>
    ```
2. 类名数组中使用**三元表达式**，按需为元素添加某些类名
   ```html
   <p :class="['thin', flag ? 'red' : '']">彬哥最帅a a a</p>
   ```
3. 应用场景
   + 【案例】网页开关灯



### 8.10 Vue指令之`v-for`和`:key`属性

> 语法 `<li v-for="(循环的item项, 索引) in 数组"></li>`

1. 迭代数组
 + 普通数组（一般般）
 + 对象数组（用的最多）
2. 迭代对象中的属性（用的不多）
3. 迭代数字（几乎没人用）

> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，`:key` 现在是必须的。

+ 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

+ 为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。



### 8.11 Vue指令之`v-if`和`v-show`

+ v-if 和 v-show 的作用，都是切换界面上元素的显示或隐藏的；

> 一般来说，v-if 有更高的**切换消耗** 而  v-show 有更高的**初始渲染消耗**。

> 因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。
+ 根据应用场景区分：
> 举例：袜子要不要洗的问题



## 9. 修饰符【了解】

### 9.1 事件修饰符

+ `.prevent` 阻止默认行为
+ `.once` 只触发1次
+ `.stop` 阻止冒泡
+ `.self` 只有在当前元素上触发事件的时候，才会调用处理函数

### 9.2 按键修饰符

按键修饰符都是配合文本输入框使用的；

+ `.enter`
+ `.tab`
+ `.esc`
+ 案例需求：用户输入密码之后，按 `enter` 键登录

## 10. 品牌管理案例

### 添加新品牌
### 删除品牌
### 根据条件筛选品牌
1. 1.x 版本中的filterBy指令，在2.x中已经被废除：
[filterBy - 指令](https://v1-cn.vuejs.org/api/#filterBy)
```html
<tr v-for="item in list | filterBy searchName in 'name'">
  <td>{{item.id}}</td>
  <td>{{item.name}}</td>
  <td>{{item.ctime}}</td>
  <td>
    <a href="#" @click.prevent="del(item.id)">删除</a>
  </td>
</tr>
```
2. 在2.x版本中[手动实现筛选的方式](https://cn.vuejs.org/v2/guide/list.html#显示过滤-排序结果)：
+ 筛选框绑定到 VM 实例中的 `searchName` 属性：
```html
<hr> 输入筛选名称：
<input type="text" v-model="searchName">
```
+ 在使用 `v-for` 指令循环每一行数据的时候，不再直接 `item in list`，而是 `in` 一个 过滤的methods 方法，同时，把过滤条件`searchName`传递进去：
```html
<tbody>
      <tr v-for="item in search(searchName)">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.ctime}}</td>
        <td>
          <a href="#" @click.prevent="del(item.id)">删除</a>
        </td>
      </tr>
    </tbody>
```
+ `search` 过滤方法中，使用 数组的 `filter` 方法进行过滤：
```js
search(name) {
  return this.list.filter(x => {
    return x.name.indexOf(name) != -1;
  });
}
```



## 作业

1. 跑马灯效果
2. 加减乘除计算器
3. 网页开关灯效果
4. 品牌列表案例



## 相关文章

1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [Vue.js双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html)