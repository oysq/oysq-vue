# oysq-vue

---

#### Vue 实例和组件

> component.html

* 所有的 vue 组件都是一个 vue 实例
* 一个 vue 应用通过 `new Vue()` 创建一个根实例，然后底下挂载、嵌套、复用很多组件实例构成一颗树来完成
* 到我们访问 app.a 时，默认访问的是 data 里的 property，如果要访问其他属性，需要加上 `$` 符号，比如：`app.$data.a` / `app.$destory()` / ...
* data 属性内的 property 是响应式的，但是只有在实例创建时就存在的属性才是响应式的，如果在实例创建后才添加的新属性，比如：`app.$data.b='123'`，是不会和更新到视图的，也可以通过 `Object.freeze()` 方法阻止某个属性的响应

#### Vue 生命周期

> lifeCycle.html

* vue 生命周期函数就是 vue 实例在某一个时间点会执行的函数
* 注意：这些函数不是放在 methods 里，而是和 methods 同级的


#### 模板语法

> template.html

* 当看到 `{{}}` 或者 `v-` 符号时，内部的值就不是存粹的字符串，而是 `js` 代码，是可以填入表达式的
* `v-text` 与 `v-html` 的区别为后者会解析为 `html`，而 `{{}}` 和 `v-text` 是相同的



