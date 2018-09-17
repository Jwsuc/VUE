# Vue.js - day6



## 今天主要内容
1. 自定义指令
2. 组件中的slot插槽
3. element-ui的使用
4. 重构品牌列表案例



## 1. 自定义指令

### 1.1 全局自定义指令

- 概念：在全局任何组件中都可以被调用的自定义指令，叫做全局自定义指令；
- 语法：`Vue.directive('全局自定义指令名称', { /* 自定义指令配置对象 */ })`

### 1.2 私有自定义指令

- 概念：只有指令所属的组件中，才可以被正常调用的指令，叫做私有自定义指令；

- 语法：

  ```vue
  // test1.vue 组件
  <template></template>
  
  <script>
      export default {
          directives: {
              '私有自定义指令名称': { /* 私有自定义指令配置对象 */ }
          }
      }
  </script>
  ```

### 1.3 指令配置对象中 `bind` 和 `inserted` 的区别

- bind 方法：
  - 绑定当前指令的元素，在内存中被创建的时候就会被立即调用；
  - 推荐把样式相关的设置，都定义在 bind 方法中；
- inserted 方法：
  - 绑定当前指令的元素，被插入到DOM树之后才会被调用；
  - 推荐把行为相关的设置，都定义在 inserted 方法中；
- 演示 bind 和 inserted 的区别：
  - 在终端中打印 `el.parentNode` 即可；



## 2. 插槽

定义：定义子组件的时候，在子组件内部刨了一个坑，父组件想办法往坑里填内容；



### 2.1 单个插槽（匿名插槽）

1. 定义插槽：在子组件作用域中，使用 `<slot></slot>` 定义一个插槽；
2. 使用插槽：在父作用域中使用带有插槽的组件时，组件内容区域中的内容，会插入到插槽中显示；
3. 注意：在一个组件的定义中，只允许出现一次匿名插槽



### 2.2 多个插槽（具名插槽）

1. 定义具名插槽：使用 `name` 属性为 `slot` 插槽定义具体名称；`<slot name="header"></slot>`
2. 使用具名插槽：在父作用域中使用带有命名插槽的组件时，需要为内容指定 `slot="插槽name"` 来填充到指定名称的插槽；



### 2.3 作用域插槽【比较重要】

1. 定义作用域插槽：在子组件中，使用 `slot` 定义插槽的时候，可以通过 `属性传值` 的形式，为插槽传递数据，例子：`<slot text="hello world" :msg="sonMsg" row="rowData"></slot>`
2. 使用作用域插槽：在父作用域中，通过定义 `slot-scope="scope"` 属性，接收并使用 插槽数据；
3. 注意：同一组件中不同插槽的作用域，是独立的！



## 3. 在项目中安装和使用`element-ui`

> Vue中比较常用的组件库： element-ui，mint-ui，Vant，iView.....

1. element-ui 是 饿了么 前端团队，开源出来的一套 Vue 组件库；

2. 完整引入 `Element-UI` 组件：【开发中使用完整引入】

   1. 运行 `npm i element-ui -S` 安装组件库

   2. 在 `index.js` 中，导入 `element-ui` 的包、配套样式表、并且安装到Vue上：

      ```js
      // 导入 element-ui 这个包
      import ElementUI from 'element-ui'
      // 导入 配套的样式表
      import 'element-ui/lib/theme-chalk/index.css'
      
      // 把 element-ui 安装到 Vue 上
      Vue.use(ElementUI)
      ```

3. 按需导入和配置 `Element-UI` ：【开发中暂时不需要，开发完毕后再按需引入】

   1. 运行 `npm i babel-plugin-component -D` 安装支持按需导入的模块；

   2. 打开 `.babelrc` 配置文件，修改如下：

      ```json
      {
        "presets": ["@babel/preset-env"],
        "plugins": ["@babel/plugin-transform-runtime", "@babel/plugin-proposal-class-properties", 
         + [
         + "component",
         + {
         +   "libraryName": "element-ui",
         +   "styleLibraryName": "theme-chalk"
         + }
        ]]
      }
      ```


## 4. 使用`vue-cli`快速初始化`vue`项目

```cmd
$ npm install -g vue-cli        // 全局安装脚手架工具
$ vue init webpack my-project   // 初始化项目
$ cd my-project                 // 切换到项目根目录中
$ npm install                   // 安装依赖包
$ npm run dev                   // 一键运行项目
```



## 5. ESLint 语法检查规范

1. 声明但是未使用的变量会报错
2. 空行不能连续大于等于2
3. 在行结尾处，多余的空格不允许
4. 多余的分号，不允许
5. 字符串要使用单引号，不能使用双引号
6. 在方法名和形参列表的小括号之间，必须有一个空格
7. 在单行注释的 // 之后，必须有一个空格
8. 在每一个文件的结尾处，必须有一个空行
9. import语句 必须放在最顶部
10. ect.......



## 6. 如何配置VSCode帮我们自动把代码格式为需要的样子

1. 在安装  `Vetur` 插件

2. 安装 `Prettier - Code formatter ` 插件

3. 打开 vs code 的 `文件 -> 首选项 -> 设置`，在`用户设置`最底部的合适位置，添加如下配置：

   ```json
   // 使用 ESLint 规则
   "prettier.eslintIntegration": false,
   // 每行文字个数超出此限制将会被迫换行
   "prettier.printWidth": 100,
   // 使用单引号替换双引号
   "prettier.singleQuote": true,
   // 格式化文件时候，不在每行结尾添加分号
   "prettier.semi": false,
   // 设置 .vue 文件中，HTML代码的格式化插件
   "vetur.format.defaultFormatter.html": "prettier"
   ```



## 7. 使用 `element-UI` 重构品牌列表案例



## 相关文章

1. 清楚Vue全家桶：

   vue + vue-router + vuex + webpack + axios/vue-resource + element-ui

2. vue-cli@3的官网：https://cli.vuejs.org/zh/

