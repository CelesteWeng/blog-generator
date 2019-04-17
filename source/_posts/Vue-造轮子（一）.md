---
title: Vue_造轮子（一）
date: 2019-04-11 21:16:37
tags: 
- 相关资料
- 构建项目流程
---

# 简易 UI 框架

## 课外资料

1. UI
	- [Framework7](https://framework7.io/docs/button.html)
	- [Ant Design](https://ant.design/docs/react/introduce-cn)
	- [Element UI](http://element.eleme.io/#/zh-CN/component/installation)
	- [iView](https://www.iviewui.com/docs/guide/install)
	
2. 设计
	- 《写给大家看的设计书》
	- [Sketch中文网](Sketch中文网)

3. ES 6
	- [ES 6 新特性列表及教程](https://frankfang.github.io/es-6-tutorials/)
	- [《ES6 深入浅出》课程](https://xiedaimala.com/courses/12a78a03-35f9-42ea-9b37-540540460f6e#/common)

4. 配色网站推荐
	- https://colorhunt.co/
	- https://color.adobe.com/explore/
	- https://www.materialpalette.com/

5. Windows 上像 Sketch 一样的工具 https://www.adobe.com/cn/products/xd.html

## 构建流程

1. 本地创建目录
2. 创建仓库（e.t. github, gitlab）
3. 相关命令：本地初始化仓库、npm、安装 parcel，不要忘记 `.gitignore`。

   ```
    touch README.md
    git init
    git add .
    git re..... // 直接复制就行

    npm init

    npm i -D parcel-bundler

    // ./node_modules/.bin/parcel index.html --no-cache

    npx parcel index.html --no-cache
   ```

4. 用 Vue.js 需要在 package.json 中配置
5. 
   ```
    "alias": {
      "vue" : "./node_modules/vue/dist/vue.common.js"
    }
   ```

6. parcel
  i. 创建 src 目录，将 JS 放入该目录
  ii. 修改代码

  ```js
  // path: 'ProjectName/src/app.js'

  import Vue from 'vue'
  import Button from './button'

  Vue.component('g-button', Button)

  new Vue({
      el: '#app'
  })
  ```

  ```
  // path: 'ProjectName/src/button.vue'
  
  <template>
    <button class="g-button">按钮</button>
  </template>
  <script>
      export default {}
  </script>
  <style lang="scss">
      .g-button {
          font-size: var(--font-size);
          height: var(--button-height);
          padding: 0 1em;
          border-radius: var(--border-radius);
          border: 1px solid var(--border-color);
          background: var(--button-bg);
          &:hover {
              border-color: var(--border-color-hover);
          }
          &:active {
              background-color: var(--button-active-bg);
          }
          &:focus {
              outline: none;
          }
      }
  </style>
  ```

## 提交

查看提交历史

1. 安装 `npm i -g git-open`
2. 在目录运行 `git open`，直接打开 github 对应页面

## Vue 知识点

### slot 插槽

1. 不能加class，可用标签包起来
2. 出现 Build Error，用标签包起来

### props 传参

```js
// props: ['icon', 'iconPosition']
props: {
    icon: {},
    iconPosition: {
        type: String,
        default: 'left',
        validator(value) {
            return value === 'left' && value === 'right'
        }
    }
}
```

### $emit

在模版中不需要写 `this`

```html
<button @click="$emit('yyy')"></button>
```

```js
<button @click="xxx"></button>

methods: {
    xxx() {
        this.$emit('yyy')
    }
}
```

### mounted

当改组件出现在页面的时候，会触发的一个函数

### $children

vue 的 $children 属性只会去找 vue 的实例，所以 div 等标签会被忽略

### $el.children

和 $children 相比，该属性可以找到 vue 实例之外的标签

### 遍历 node

```js
// 并非 for in
for(let node of xxx) {}
```

### 全局 API

#### Vue.expend(options)

options 为包含组件选项的对象

## 单元测试

- BDD Behavior-Driven Development(行为 驱动 开发)
- TDD Test-Driven Development(测试 驱动 开发)
- Assert 断言(我主观认为，`consolo.assert(xxx)` 就是断言，若 xxx 为假则会报错)

### Chai.js

让断言更加方便，提供三种方法：

1. Should
2. Expect
3. Assert

#### chai.spies

mock，单元测试，断言，监听事件时可用