# oysq-vue

---

> 这是一篇对 Vue 零散知识点的扫盲和代码验证~~

#### Vue 实例和组件

> component.html

* 所有的 vue 组件都是一个 vue 实例
* 一个 vue 应用通过 `new Vue()` 创建一个根实例，然后底下挂载、嵌套、复用很多组件实例构成一颗树来完成
* 我们访问 `app.a`（或 `this.a`）时，默认访问的是 data 里的 property，如果要访问其他属性，需要加上 `$` 符号，比如：`app.$data.a` / `app.$destory()` / ...
* data 属性内的 property 是响应式的，但是只有在实例创建时就存在的属性才是响应式的，如果在实例创建后才添加的新属性，比如：`app.$data.b='123'`，是不会和更新到视图的，也可以通过 `Object.freeze()` 方法阻止某个属性的响应

#### 生命周期

> life_cycle.html

* vue 生命周期函数就是 vue 实例在某一个时间点会执行的函数
* 注意：这些函数不是放在 methods 里，而是和 methods 同级的


#### 模板语法

> template.html

* 当看到 `{{}}` 或者 `v-` 或者 `:` 符号时，内部的值就不是存粹的字符串，而是 `js` 代码，是可以填入表达式的
* `v-text` 与 `v-html` 的区别为后者会解析为 `html`，而 `{{}}` 和 `v-text` 是相同的


#### 计算属性、方法、侦听器

> computed.html

* computed 是自带缓存的，当它计算所需要依赖的变量没有改变时，它是不会重新计算的。而方法的方式，每次页面重新渲染，它都会重新计算一次
* 侦听器 `watch` 也是在指定的变量发生改变时触发，可以实现类似的缓存效果，但是所有依赖的变量都得写一个侦听器，并不适合此场景
* computed 还支持细化的 `get`/`set` 方法，注意 `set` 方法里面可能会改变多个依赖的 property，但是 `get` 方法只会触发一次
* `{{}}` 符号内使用 property 和 computed 是不需要带 `()` 后缀的，但是如果使用函数（methods 里的方法），就需要带上 `()` 后缀


#### 样式绑定

> bind_style.html

* `:class="{activated: isActivated}"` : isActivated=true时，:class="activated"，否则 class=""
* `:class="[classStyle1, classStyle2, ...]"` : classStyle1、classStyle2 是一个个的 data 变量，值是什么，样式就是什么
* `:style=styleObj` / `:style=[styleObj1, styleObj2]` : 也是类似的道理，styleObj 是一个变量，里面存的不是class的名称，而是class内的各种样式

#### 条件渲染

> condition.html

* `v-if` 标签决定一个标签是否在页面上渲染，不渲染时，连代码都不存在
* `v-show` 标签决定一个标签是否在页面上展示，不展示时，代码还是存在的，只是通过 `style="display: none;"` 来隐藏。
* 频繁显示隐藏的情况下，`v-show` 的性能更高
* `v-if`/`v-else-if`/`v-else` 三个标签要连着用，中间不能穿插其他的标签，否则会报错
* vue在切换



















