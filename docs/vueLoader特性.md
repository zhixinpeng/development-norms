# VueLoader特性

> vue-cli创建的项目主要依赖vue-loader来进行编译，了解vue-loader的特性很重要。

## ES2015

当项目中配置了 `babel-loader` 或者 `buble-loader`，`vue-loader` 会使用他们处理所有 `.vue` 文件中的 `<script>` 部分，允许我们在 `Vue` 组件中使用 `ES2015`，如果你还没有使用 ES2015，你应该使用它，这有一些不错的学习资源：

- [ECMAScript 6 入门](http://es6.ruanyifeng.com/)
- [ES6 Features](https://github.com/lukehoban/es6features)

掌握了ES6之后我们可以在项目中使用方便快捷的ES6语法。

但是，注意，不是所有的最新的ES6甚至ES7、ES8语法都能满足我们的兼容性要求。

如果考虑到兼容IE浏览器（IE9+），我们应该避免使用一些最新特性（例如异步函数async及装饰器@decoration）。

具体的使用方案应该根据项目需求来，最新的不一定是最好的。

## 使用 .babelrc 配置 Babel

ES6需要使用babel（垫片）来编译，以便我们的项目可以在更多的浏览器中运行，当然因为Vue.js仅支持IE9+的原因，一般我们只需要考虑现代浏览器中的运行结果。

`babel-loader` 遵守 `.babelrc`，因此这是配置 `Babel presets` 和插件推荐的方法。这个文件在我们的项目根目录中可以找到并进行配置

一般为了兼容IE浏览器，我们需要添加一些垫片，比如 `babel-polyfill`、`es6-promise`等等。

## CSS作用域

当 `<style>` 标签有 `scoped` 属性时，它的 `CSS` 只作用于当前组件中的元素。这类似于` Shadow DOM` 中的样式封装。它有一些注意事项，但不需要任何 `polyfill`。它通过使用 `PostCSS` 来实现以下转换：

```html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

转换结果：

```html
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

## 混用本地和全局样式

你可以在一个组件中同时使用有作用域和无作用域的样式：

```html
<style>
/* 全局样式 */
</style>

<style scoped>
/* 本地样式 */
</style>
```

## 子元素的根元素

使用 `scoped` 后，父组件的样式将不会渗透到子组件中。不过一个子组件的根节点会同时受其父组件有作用域的 `CSS` 和子组件有作用域的 `CSS` 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

## 深度作用选择器

如果你希望 `scoped` 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 `>>>` 操作符：

```html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

上面代码将会变异成：

```css
.a[data-v-f3f3eg9] .b { /* ... */ }
```

有些像 `SASS` 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 操作符取而代之——这是一个 `>>>` 的别名，同样可以正常工作。

## 动态生成的内容

通过 `v-html` 创建的 DOM 内容不受作用域内的样式影响，但是你仍然可以通过深度作用选择器来为他们设置样式。

## 使用预处理器

在 webpack 中，所有的预处理器需要匹配对应的 `loader`。`vue-loader` 允许你使用其它 `webpack loader` 处理 `Vue` 组件的某一部分。它会根据 `lang` 属性自动推断出要使用的 `loader`。

在项目中，我们推荐使用预处理器来处理样式，这很提高我们的代码整洁性和模块化，而且可读性高

## 资源路径处理

默认情况下，`vue-loader` 使用 `css-loader` 和 `Vue` 模版编译器自动处理样式和模版文件。在编译过程中，所有的资源路径例如 `<img src="...">`、`background: url(...)` 和 `@import` 会作为模块依赖。

例如，`url(./image.png)` 会被转换为 `require('./image.png')`，而

```html
<img src="../image.png">
```

将会编译为：

```javascript
createElement('img', { attrs: { src: require('../image.png') }})
```

## 编译规则

- 如果路径是绝对路径，会原样保留。
- 如果路径以 `.` 开头，将会被看作相对的模块依赖，并按照你的本地文件系统上的目录结构进行解析。
- 如果路径以 `~` 开头，其后的部分将会被看作模块依赖。这意味着你可以用该特性来引用一个 node 依赖中的资源：
  ```html
  <img src="~/some-npm-package/foo.png">
  ```
- `(13.7.0+)` 如果路径以 `@` 开头，也会被看作模块依赖。如果你的 `webpack` 配置中给 `@` 配置了 `alias`，这就很有用了。所有 `vue-cli` 创建的项目都默认配置了将 `@` 指向 `/src`。同样的，如果我们配置了 `assets` 指向 `src/assets`，我们就可以使用 `~assets`来直接指向资源目录，这是一个很方便的做法，可以避免使用 `../../`的极大不便。