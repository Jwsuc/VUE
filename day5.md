# Vue.js - day5



## 0. 今天主要学习目标

1. 单页面应用程序 和 实现原理
2. **vue路由的 基本安装配置 和 使用**
3. watch监听 和 computed 计算属性
4. **vue-cli 脚手架**



## 1. 使用 `<component>`标签实现组件切换

1. `<component>` 是Vue提供的；作用是 把 is 指定的 `组件名称`，渲染到 ``<component>`` 内部
2. 身上有一个 `:is`属性



## 2. SPA单页应用

### 2.1 锚链接及常规url的区别

1. 普通的URL地址：会刷新整个页面；会追加浏览历史记录；
2. 锚链接：不会触发页面的整体刷新；会追加浏览历史记录；（锚链接是页面内的跳转）



### 2.2 什么是SPA,为什么有SPA

- 概念定义：SPA英文全称是`Single Page Application`, 中文翻译是 “单页面应用程序”；
- 通俗的理解是：只有一个Web页面的网站；网站的所有功能都在这个唯一的页面上进行展示与切换；
- 特点：
  - 只有一个页面 
  - 浏览器一开始请求这个页面，必须加载对应的HTML， CSS， JavaScript
  - 用户的所有操作，都在这唯一的页面上完成
  - 页面数据都是用Ajax请求回来的
- 好处：
  - 实现了前后端分离开发，各司其职；提高了开发效率；
  - 用户体验好、快，内容的改变不需要重新加载整个页面；
- 缺点：
  + 对SEO不是很友好，因为页面数据是Ajax渲染出来的； (SSR)服务器端渲染；
  + 刚开始的时候加载速度可能比较慢；项目开发完毕之后，可以单独对首屏页面的加载时间做优化；
  + 页面复杂度比较高，对程序员能力要求较高；



### 2.3 原生实现SPA

使用 component 标签的`:is`属性来切换组件

总结：单页面应用程序中，实现组件切换的根本技术点，就是 监听 window.onhashchange 事件；



## 3. 路由

### 3.1 什么是路由

什么是路由：路由 就是 对应关系；

1. 后端路由的定义：URL地址 到 后端 处理函数之间的关系；
2. 前端路由的定义：hash 到 组件 之间的对应关系；
3. 前端路由的目的：为了实现单页面应用程序的开发；
4. 前端路由的三个组成部分：
   + 链接
   + 组件
   + 链接 和 组件之间的对应关系



### 3.2 在 vue 中使用 vue-router【重点】

1. 安装导入并注册路由模块：

   + 运行 `npm i vue-router -S` 安装路由模块

   + 在 `index.js` 中导入并注册路由模块

     ```js
     // 导入路由模块
     import VueRouter from 'vue-router'
     // 注册路由模块
     Vue.use(VueRouter)
     ```

2. 创建路由链接：

   ```html
   <!-- router-link 就是 第一步，创建 路由的 hash 链接的 -->
   <!-- to 属性，表示 点击此链接，要跳转到哪个 hash 地址， 注意：to 属性中，大家不需要以 # 开头 -->
   <router-link to="/home">首页</router-link>
   <router-link to="/movie">电影</router-link>
   <router-link to="/about">关于</router-link>
   ```

3. 创建并在 `index.js` 中导入路由相关的组件：

   ```js
   import Home from './components/Home.vue'
   import Movie from './components/Movie.vue'
   import About from './components/About.vue'
   ```

4. 创建路由规则

   ```js
   // 创建路由规则（对应关系）
   const router = new VueRouter({ // 配置对象中，要提供 hash 地址 到 组件之间的 对应关系
     routes: [ // 这个 routes 就是 路由 规则 的数组，里面要放很多的对应关系
       // { path: 'hash地址', component: 配置对象 }
       { path: '/home', component: Home },
       { path: '/movie', component: Movie },
       { path: '/about', component: About }
     ]
   })
   
   // 创建的 router 对象，千万要记得，挂载到 vm 实例上
   const vm = new Vue({
     el: '#app',
     render: c => c(App),
     router // 把 创建的路由对象，一定要挂载到 VM 实例上，否则路由不会生效
   })
   ```

5. 在页面上放路由容器

   ```html
   <!-- 这是路由的容器，将来，通过路由规则，匹配到的组件，都会被展示到这个 容器中 ，相当于一个占位符 -->
   <router-view></router-view>
   ```


### 3.3 路由规则的匹配过程

1. 用户点击 页面的 路由链接`router-link`，点击的一瞬间，就会修改 浏览器 地址栏 中的 Hash 地址；
2. 当 hash 地址被修改以后，会立即被  `vue-router` 监听到，然后进行 路由规则的 匹配；最终，找到 要显示的组件；
3. 当 路由规则匹配成功以后，就找到了 要显示的 组件，然后 把 这个组件，替换到 页面 指定的 路由容器`router-view` 中



### 3.4 设置路由高亮的两种方式

1. 通过路由默认提供的`router-link-active`, 为这个类添加自己的高亮样式即可；
2. 通过路由构造函数，在传递路由配置对象的时候，提供 `linkActiveClass` 属性，来覆盖默认的高亮类样式；



### 3.5 嵌套路由（子路由）

1. 在对应的路由组件中，新增 `router-link` 路由链接；

2. 创建 `router-link` 对应要显示的组件；

3. 在对应的路由规则中，通过 `children` 属性，定义子路由规则：

   ```js
   {
     path: '/movie',
     component: Movie,
     redirect: '/movie/in_theaters',
     // 指定子路由规则
     children: [
       { path: '/movie/in_theaters', component: In_theaters },
       { path: '/movie/coming_soon', component: Coming_soon }
     ]
   }
   ```

4. 在 `router-link` 之下，添加 `router-view` 容器组件；



### 3.6 路由传参

1. 路由传参，首先，要把路由规则中，对应的参数位置，通过 : 来定义为一个参数；

2. 可以在组件中，直接使用`this.$route.params` 来获取参数；【写起来太麻烦，不推荐】

3. 也可以开启路由的 props 传参，来接收路由中的参数；【推荐方式】

   1. 在需要传参的路由规则中，添加 `props: true`

      ```js
      { path: '/movie/:type/:id', component: movie, props: true }
      ```

   2. 在 对应的组件中，定义 props 并接收路由参数

      ```js
      const movie = {
            template: '<h3>电影组件 --- {{type}} --- {{id}}</h3>', // 使用参数
            props: ['type', 'id'] // 接收参数
          }
      ```



### 3.7 命名路由

什么是命名路由： 就是为路由规则，添加了一个 name ;

1. 什么是命名路由

2. `router-link`中通过路由名称实现跳转

3. 命名路由使用 `params`属性传参

   ```html
   <router-link :to="{ name: 'moviePage', params: {type: 'top250', id: 9} }" tag="li">
   ```



### 3.8 编程式（JS）导航

之前所学的`router-link`是标签跳转；
除了使用`router-link`是标签跳转之外，还可以使用`Javascript`来实现路由的跳转；

1. 什么是编程式导航
   - 使用`vue-router`提供的JS API实现路由跳转的方式，叫做编程式导航；
2. 编程式导航的用法

- `this.$router.push('路径的地址')`
- `this.$router.go(n)`
- `this.$router.forward()`
- `this.$router.back()`

> 今天，学了好多 router 之类的东西，其中
>
> - this.$route  是 路由参数对象
> - this.$router 是路由导航对象
> - vm 实例上的 router 属性，是来挂载路由对象的
> - 在 new VueRouter({ /* 配置对象 */ }) 的时候，配置对象中，有一个 routes 属性， 是来创建路由规则的



### 3.9 路由导航守卫

- 案例需求：只允许登录的情况下访问 `后台首页` ，如果不登录，默认跳转回登录页面；

- API语法：

  ```js
  // 参数1：是要去的那个页面路由相关的参数
  // 参数2：从哪个页面即将离开
  // 参数3：next 是一个函数，就相当于 Node 里面 express 中的 next 函数
  // 注意： 这里的 router 就是 new VueRouter 得到的 路由对象
  router.beforeEach((to, from, next) => { /* 导航守卫 处理逻辑 */ })
  ```


## 4. `watch` 监听

+ watch 监听的特点：监听到**某个数据**的变化后，**侧重于做某件事情**；
  + 只要被监听的数据发生了变化，会自动触发 watch 中指定的处理函数；

- 案例：登录  `密码` 的长度检测
- 密码长度小于8位字体为红色；大于等于8位字体为黑色；



## 5. `computed` 计算属性

+ 计算属性特点：同时监听**多个数据**的变化后，**侧重于得到一个新的值**；
  + 只要依赖的任何一个数据发生了变化，都会自动触发计算属性的重新求值；
+ 名称案例



## 6. 使用 vue-cli 快速创建 vue 项目

+ 为什么要使用 `vue-cli` 创建项目：
  + 在终端运行一条简单的命令，即可创建出标准的 vue 骨架项目；
  + 不必自己手动搭建 vue 项目基本结构，省时省力；
  + 不必关心 webpack 如何配置，只关注于项目代码的开发；

+ 安装 `vue-cli` 

  ```cmd
  $ npm install -g vue-cli        // 全局安装脚手架工具
  ```

+ 初始化项目：

  ```cmd
  $ vue init webpack my-project   // 初始化项目
  $ cd my-project                 // 切换到项目根目录中
  $ npm install                   // 安装依赖包
  $ npm run dev                   // 一键运行项目
  ```



## 7. `webpack` 中 `省略文件后缀名` 和配置 `@` 路径标识符

1. 省略文件扩展名：

   + 打开 `webpack.config.js`，在导入的配置对象中，新增 resolve 节点；

   + 在 resolve 节点中，新增 `extensions` 节点：

     ```js
     resolve: {
         // resolve 节点下的 extensions 数组中，可以配置，哪些扩展名可以被省略
         extensions: ['.js', '.vue', '.json']
     }
     ```

   + 修改完配置以后，重新运行 `npm run dev` 查看效果；

2. 配置 `@` 指向 `src` 目录：

   ```js
   resolve: {
       alias: {
         '@': path.join(__dirname, './src') // 让 @ 符号，指向了 项目根目录中的 src
       }
     }
   ```



## 拓展

1. 全局的包，默认都安装到了 `C:\Users\自己的用户名文件夹\AppData\Roaming\npm`

2. 安装包的时候，默认会把包缓存到 `C:\Users\自己的用户名文件夹\AppData\Roaming\npm-cache` 目录中

3. 如果 大家使用 `npm install` 装包遇到如下错误，可以尝试删除 `npm-cache` 缓存目录：

   ![](./下包报错.png)

