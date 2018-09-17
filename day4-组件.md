# Vue.js - day4



## 今天主要内容

1. 在webpack中使用vue
2. 组件和.vue文件
3. 组件之间数据通信



## 1. 在 webpack 中安装和配置 vue

1. 运行 `npm i vue -S` 把 vue 安装到项目依赖

2. 在 `index.js` 中使用 import 导入 vue 模块：

   ```js
   import Vue from 'vue'
   ```

3. 在 `index.html` 中创建将来要被 vm 实例控制的 div：

   ```html
   <!-- 将来，这个 div 就是 Vue实例 要控制的区域 -->
   <div id="#app"></div>
   ```

4. 在 `index.js` 中创建 vm 实例，并使用 el 指定要控制的区域，使用 data 指定要渲染的数据：

   ```js
   const vm = new Vue({
     el: '#app', // 要控制的区域
     data: {
       msg: 'ok' // 要渲染的数据
     }
   })
   ```

   

## 2. 为什么在基于 webpack 的 vue项目中, 按照如上操作会报错呢

1. 因为使用 `import` 导入的 `vue` 模块，导入的并不是功能最全的 vue 包；而是删减版的；
2. 删减版的 vue 包中功能很少，目的是为了减小vue 包的体积，因为文件越小，网络请求越快！
3. **如何让 `import` 导入最全的 `vue` 包呢？**
   + 把 `import Vue from 'vue' ` 改写为 `import Vue from 'vue/dist/vue.js' `
4. **注意：**在学习阶段，可以暂时 `import` 最全的 `vue` 包；**等开发项目的时候，一定要使用 删减版的 包**；



## 3. 定义Vue组件



### 3.1 `模块化`和`组件化`概念的解读

1. **什么是模块化：**`是从代码的角度`分析问题；把可复用的代码，抽离为单独的模块；
   + 模块化的好处：
     + 提供模块作用域的概念，防止全局变量污染；
     + 提高了代码的复用率，方便了程序员之间 共享代码；
2. **什么是组件化：**组件化是从`页面UI的角度`进行分析问题的；把页面中可复用的UI结构，抽离为单独的组件；
   + 组件化的好处：
     + 方便了UI结构的重用；
     + 随着项目开发的深入，手中可用的组件会越来越多；
     + 可以直接使用第三方封装好的组件库；
     + 组件化能够让程序员更专注于自己的业务逻辑；



### 3.2 定义全局组件

1. 定义组件的语法
   - `Vue.component('组件的名称', { 组件的配置对象 })`
   - 在组件的配置对象中：可以使用 `template` 属性指定当前组件要渲染的模板结构；
2. 使用组件的语法
   - 把 `组件的名称`, 以标签的形式，引入到页面上就行；
3. **注意：**
   + 从更抽象的角度来说，每个组件，就相当于是一个**自定义的元素**；
   + 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！



### 3.3 组件中定义`data数据`、`methods方法`以及`生命周期函数`

可以认为：**组件是特殊的Vue实例**；

组件和实例的相同和区别：

1. 组件中的 data 必须是一个 function 并 return 一个 字面量对象；在 Vue 实例中，实例的 data 既可以是 对象，可以是 方法；
2. 组件中，直接通过 template 属性来指定组件的UI结构；在 Vue 实例中，通过 el 属性来指定实例控制的区域；但是实例也可以使用 `template`;
3. 组件和实例，都有自己的`生命周期函数`，`私有的过滤器`，`methods` 处理函数；



### 3.4 为什么组件中的 data 属性必须定义为一个方法并返回一个对象

1. 通过计数器案例演示
   总结： 因为这样，能够保证每次创建的组件实例，都有自己的一块唯一的数据内存，防止组件之间数据的干扰；



### 3.5 使用 `components` 属性定义私有子组件

1. 在每个vue实例中，和 `el, data, methods, filters` 平级，可以使用 `components` 属性，注册当前实例的私有子组件；
2. 和 `Vue.component()` 注册的全局组件不同，私有组件，只能在当前实例控制的区域内使用！



## 4. `.vue` 文件的结构说明

+ 为什么要把组件，单独的定义到 `.vue` 文件中？
  + 之前创建组件太麻烦，没有智能提示和代码高亮；
  + 之前定义组件，和其它JS代码逻辑掺杂在一块儿，代码不易维护，没有把组件化的优势发挥到极致！

- 每个 .vue 文件，都是一个 vue 组件，它由三部分组成：
  1. `template` 结构
  2. `script` 行为
  3. `style` 样式



## 5. 在`webpack`中配置`.vue`组件页面的解析

1. 运行`npm i vue-loader vue-template-compiler -D`

2. 添加rules匹配规则：

   ```js
   { test: /\.vue$/, use: 'vue-loader' }
   ```

3. 在`webpack.config.js`中导入并配置插件：

   ```js
   // 导入插件
   const VueLoaderPlugin = require('vue-loader/lib/plugin')
   // new 一个插件的实例对象
   const vuePlugin = new VueLoaderPlugin()
   
   // 把 new 出来的插件实例对象，挂载到 `plugins` 节点中：
   plugins: [...其它插件, vuePlugin]
   ```



## 6. 导入并使用 .vue 组件的两种方式

1. 全局注册 `.vue` 文件：
   + 在 `index.js` 中导入 `.vue` 组件
   + 使用 `Vue.component()` 把导入的 `.vue` 组件，注册为全局组件
2. 私有注册 `.vue` 文件：
   + 定义两个独立的组件 `a.vue` 和 `c.vue`
   + 在 `a.vue` 组件中，导入 `c.vue` 文件，并使用 `a.vue` 组件的 `components` 属性，把 `c.vue` 注册为自己的私有子组件

### 

## 7. 组件之间的数据通信

### 7.1 父组件向子组件传递普通数据

1. 在父组件中，以标签形式使用子组件的时候，可以通过属性绑定，为子组件传递数据：

   ```html
   <my-son :pmsg1="parentMsg" :pinfo="parentInfo"></my-son>
   ```

2. 在子组件中，如果向用父组件传递过来的数据，必须先定义 `props` 数组来接收：

   ```js
   <script>
     export default {
       data(){
         return {}
       },
       methods: {},
       // property 属性
       // 注意：父组件传递到子组件中的数据，必须经过 props 的接收，才能使用；
       // 通过 props 接收的数据，直接可以在页面上使用；注意：不接受，不能使用外界传递过来的数据
       props: ['pmsg1', 'pinfo']
     }
   </script>
   ```

3. 接收完的props数据，可以直接在子组件的 template 区域中使用：

   ```html
   <template>
     <div>
       <h3>这是子组件 --- {{pmsg1}} --- {{pinfo}}</h3>
     </div>
   </template>
   ```



### 7.2 父组件向子组件传递方法

1. 如果父向子传递方法，需要使用 事件绑定机制：

   ```html
   <my-son @func1="show"></my-son>
   ```

   其中，为 子组件传递的 方法名称为 `func1`, 具体的方法引用，为 父组件中的 show 方法

2. 子组件中，可以直接通过 `this.$emit('func1')` 来调用父组件传递过来的方法；



### 7.3 子组件向父组件传值

1. 子向父传值，要使用 `事件绑定机制@`；
2. 父向子传递一个方法的引用
3. 子组件中，可以使用 `this.$emit()` 来调用父组件传递过来的方法
4. 在使用`this.$emit()`调用 父组件中方法的时候，可以从第二个位置开始传递参数；把子组件中的数据，通过实参，传递到父组件的方法作用域中；
5. 父组件就可以通过形参，接收子组件传递过来的数据；



### 7.4 兄弟组件之间传值

> 注意：兄弟组件之间，实现传值，用到的技术，是 `EventBus`

1. 定义模块 `bus.js`

   ```js
   import Vue from 'vue'
   
   export default new Vue()
   
   ```

2. 在需要接收数据的兄弟组件中，导入 `bus.js` 模块

   ```js
   import bus from './bus.js'
   ```

3. 在需要接收数据的兄弟组件中的 `created` 生命周期函数里，

   使用 `bus.$on('事件名称', (接收的数据) => {})` 自定义事件：

   ```js
   created(){
     // 定义事件
     bus.$on('ooo', (data)=>{
       console.log(data)
     })
   }
   ```

4. 在需要发送数据的兄弟组件中，导入 `bus.js` 模块

   ```js
   import bus from './bus.js'
   ```

5. 在需要发送数据的兄弟组件中，使用 `bus.$emit('事件名称', 要发送的数据)` 来向外发送数据：

   ```js
   import bus from './bus.js'
   export default {
       data(){
         return {
           msg: 'abcd'
         }
       },
       methods: {
         sendMsg(){
           // 触发 绑定的 事件，并向外传递参数
           bus.$emit('ooo', this.msg)
         }
       }
   }
   ```

   

## 8. 使用 `this.$refs` 来获取元素和组件

1. 把要获取的DOM元素，添加 ref 属性，创建一个DOM对象的引用，指定的值，就是引用的名称：

   ```html
   <p ref="myElement11">这是父组件</p>
   ```

2. 如果要获取 某个引用所对应的 DOM对象，则直接使用 `this.$refs.引用名称`

   ```js
   console.log(this.$refs.myElement11)
   ```

3. 也可以使用 ref 为组件添加引用；可以使用 `this.$refs.组件应用名称`, 拿到组件的引用，从而调用组件上的方法 和 获取组件data上的 数据；



## 9. 使用 霸道的 render 函数渲染组件

1. 如果在 vm 实例中既指定了 `el` 又指定了 `render` 函数，则会把 el 所指的的区域，替换为 render 函数中所提供的组件；
2. 既然 render 函数会替换到 el 区域内的所有代码，也会让 template 属性失效；因此，在删减版的 vue 包中，new 出来的 Vue 实例对象，不允许 挂载 data 属性和 template 属性！
