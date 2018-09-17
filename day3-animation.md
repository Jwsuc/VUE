# Vue.js - 动画



## 今天主要内容

1. Vue.js中的过渡动画
2. webpack 的基本配置和使用
3. ES6 模块化导入和导出



## 1. [Vue中的动画](https://cn.vuejs.org/v2/guide/transitions.html)

- **为什么要有动画：**动画能够增加页面趣味性，目的是**为了让用户更好的理解页面的功能**；
- **注意：**Vue中的动画，都是简单的过渡动画，并不会有 CSS3 那么炫酷；

### 1.1 Vue中动画的基本介绍

1. 每个都动画分为两部分：
   - **入场动画**：从不可见（flag = false） -> 可见（flag = true）
   - **离场动画**：可见（flag = true） -> 不可见（flag = false）
2. 入场时候，Vue把这个动画，分成了**两个时间点**和**一个时间段**：
   - `v-enter`：入场前的样式
   - `v-enter-to`：入场完成以后的样式
   - `v-enter-active`：入场的时间段
3. 离场时候，Vue把动画，分成了**两个时间点**和**一个时间段**：
   - `v-leave`：离场之前的样式
   - `v-leave-to`：离场完成以后的样式
   - `v-leave-active`：离场的时间段



### 1.2 使用过渡类名

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


### 1.3 [使用第三方 CSS 动画库](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

1. 把需要添加动画的元素，使用v-if或v-show进行控制
2. 把需要添加动画的元素，使用Vue提供的元素 `<transition></transition>` 包裹起来
3. 为  `<transition></transition>`  添加两个属性类`enter-active-class`, `leave-active-class`
4. 把需要添加动画的元素，添加一个 `class="animated"`



### 1.4 [v-for 的列表过渡](https://cn.vuejs.org/v2/guide/transitions.html#%E5%88%97%E8%A1%A8%E8%BF%87%E6%B8%A1)

1. 把v-for循环渲染的元素，添加 `:key` 属性【注意：如果为列表项添加动画效果，一定要指定key，并且，key的值不能为索引，必须是ID】

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




### 1.5 列表的排序过渡

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