# oysq-vue

---

> 这是一篇对 Vue 零散知识点的扫盲和代码验证~~

#### Vue 实例和组件

> component.html

* 所有的 vue 组件都是一个 vue 实例
* 一个 vue 应用通过 `new Vue()` 创建一个根实例，然后底下挂载、嵌套、复用很多组件实例构成一颗树来完成
* 我们访问 `app.a`（或 `this.a`）时，默认访问的是 data 里的 property，如果要访问其他属性，需要加上 `$` 符号，比如：`app.$data.a` / `app.$destory()` / ...
* data 属性内的 property 是响应式的，但是只有在实例创建时就存在的属性才是响应式的，如果在实例创建后才添加的新属性，比如：`app.$data.b='123'`，是不会和更新到视图的，也可以通过 `Object.freeze()` 方法阻止某个属性的响应
* 子组件通过 `this.$emit('handle-click')` 触发外部方法时，方法名最好用小写加 `-` 的方式

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
* vue 在切换 `v-if`/`v-else-if`/`v-else` 三个标签的内容时会尽量去复用里面的内容，导致组件的内容没有更新，在标签上加 `key` 去避免


#### 列表渲染

> list.html

* 数组循环 `v-for="(item, index) in list"` 用 `in`
    * vue 中通过直接对list数组下标进行赋值的方式虽然能改变值，但不能渲染到页面，需要通过以下方法之一:
        * vue 提供的7种变异方法修改list，需要注意的是，当通过7种方式修改后，之前通过数组下标方式修改的值也会一并渲染到页面上。7种变异方法：`push` `pop` `shift` `unshift` `splice` `sort` `reverse`
        * 直接改变数组的引用，指向一个新数组：`list = [{}, {}, ...]`
        * 使用 `Vue.set(app.list, 2, {"a": "123"})` 或 `app.$set(app.list, 2, {"a": "123"})`
    * 当我们需要循环一整块dom元素时，可以使用 `template` 模板占位符标签包裹，模板占位符可以帮助我们包裹一些元素，但是真正渲染的时候并不会出现在页面上

* 对象循环 `v-for="(value, key, index) of user"` 用 `of`
    * 对象循环时，可以直接使用 `app.user.addr = "abc123"` 修改某个key的值或新增一个key，但是新增的key不会在页面进行渲染，修改的才会
    * 改变整个对象的引用可以修改或新增key，且都渲染到页面
    * `Vue.set(app.user, "addr", "abc123")` 或 `app.$set(app.user, "addr", "aaaaa")` 可以对修改或新增key，且都渲染到页面


#### 事件绑定

> list.html

* `@click` 可以传递 `$event` 得到点击事件的详细信息
* 点击修饰符：`@click` 支持很多事件修饰符
   * `@click.self="handkerClick"` 只有点击元素本身才会触发点击事件，用来阻止冒泡行为
   * `@click.once="handleClick"` click事件只绑定一次，意思是点击事件只会出触发一次
   * `@click.prevent` 可以阻止表单提交事件
   * `@click.capture="handleClick"` 可以将默认的冒泡规则改变为捕获原则，意思是原来是里面先执行，现在改成了外面先执行
   * `@click.left`/`@click.right`/`@click.middle` 可以指定左、右、中键才触发
* 按键修饰符：`@keydown` 监听所有的按键事件，可以通过修饰符改变为：只监听指定的按键
   * `@keydown.enter` 监听回车
   * `@keydown.esc` 监听返回
   * `@keydown.tab` 监听tab
   * `@keydown.ctrl` 同时按住ctrl+其它按键时才触发


#### 表单绑定

> model.html

* `v-model="myValue"` 标签可以用来绑定各种标签的值，如 `input` 框的输入值、`select` 标签的勾选值等等...
* 区分：
   * `v-mode` 用来绑定标签的操作值，是一个模型，所以用法是等于：`v-model="myValue"`
   * `v-bind` 用来绑定标签的属性，所以用法是谁等于谁，`v-bind:class="activated"`，因为同一个标签会有很多属性，所以可以简写为 `:class="activated`
* 修饰符
   * `v-model.lazy` 懒绑定可以使得数据等到失焦或者回车后才绑定到对应的模型上，而不是每次输入都会绑定
   * `v-model.number` 使绑定值的类型尽可能转化为 `number` 类型
   * `v-model.trim` 可以在绑定前去除前后的空格


#### 组件细节之is语法

> component_detail_1.html

* html5 的规范要求: `<table>` 标签下必须是 `<tbody>` 标签，然后依次是 `<tr>` 和 `<td>` ，但是当我们用组件封装了 `<tr>` 和 `<td>` 时，html5 会解析失败，这个时候可以用 `is=` 语法来制定这个 `<tr>` 标签其实是某个组件。类似的还有 `<ul>`/`<li>`标签、`<select>` 标签等


#### 组件细节之数据安全

> component_detail_2.html

* Vue 在根组件定义 `data` 时是直接返回一个对象的，但是在组件内，我们的 `data` 必须是一个函数，函数内返回了一个对象。之所以这么设计，是因为根组件只会被调用一次，而子组件会被调用多次，我们希望每个子组件都有自己独立使用的 data 数据，而不是所有子组件共有一分 data，因此每个子组件都单独一份数据。


#### 组件使用细节之ref引用

> component_detail_3.html

* 当 `ref="myRef"` 写在普通标签上时，`this.$refs.myRef` 指向的是一个 dom 元素
* 当 `ref="myRef"` 写在vue组件标签上时，`this.$refs.myRef` 指向的是该组件的对象本身


#### 组件间传值

> component_params.html

* 父组件通过属性(`props`)向子组件传值，子组件通过事件触发(`$emit`)的方式向父组件传值
* 单向数据流：
   * vue中不推荐子组件直接修改父组件传递过来的值（只能用，不能改）
   * 因为如果这个值是一个对象，父组件还同时把它传递给了其他组件，那修改会直接影响到其它组件
   * 如果要修改，需要在组件内的 `data` 定义一个变量，初始化为传递进来的值，后续使用、修改都只用自己定义的变量即可


















