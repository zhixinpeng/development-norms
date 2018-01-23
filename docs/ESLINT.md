# ESLINT

> ESLIN一款代码风格检查工具，它可以识别和格式化不符合要求的代码，对代码整洁美观、代码风格统一有很重要的作用，能非常有效的控制代码质量。

## 特点

- 默认规则包含所有 JSLint、JSHint 中存在的规则，易迁移；
- 规则可配置性高：可设置「警告」、「错误」两个 error 等级，或者直接禁用；
- 包含代码风格检测的规则（可以丢掉 JSCS 了）；
- 支持插件扩展、自定义规则

## 配置项

- 使用 .eslintrc 文件（支持 JSON 和 YAML 两种语法）
- 在 package.json 中添加 eslintConfig 配置块
- 直接在代码文件中定义

```json
// .eslintrc 文件示例
{
  "env": {
    "browser": true,
  },
  "globals": {
    "angular": true,
  },
  "rules": {
    "camelcase": 2,
    "curly": 2,
    "brace-style": [2, "1tbs"],
    "quotes": [2, "single"],
    "semi": [2, "always"],
    "space-in-brackets": [2, "never"],
    "space-infix-ops": 2,
  }
}
```

.eslintrc 放在项目根目录，则会应用到整个项目；如果子目录中也包含 .eslintrc 文件，则子目录会忽略根目录的配置文件，应用该目录中的配置文件. 这样可以方便地对不同环境的代码应用不同的规则

```javascript
// 代码文件内配置的规则会覆盖配置文件里的规则

// 禁用ESLINT
/* eslint-disable */
var obj = { key: 'value', }; // I don't care about IE8  
/* eslint-enable */

// 禁用一条规则
/*eslint-disable no-alert */
alert('doing awful things');  
/* eslint-enable no-alert */

// 调整规则
/* eslint no-comma-dangle:1 */
// Make this just a warning, not an error
var obj = { key: 'value', }  
```

## 工作流集成

**ESLINT可以集成到主流的编辑器和vue-cli创建的项目中**

使用vue-cli新建项目时，会有创建项目时的各种配置选项，其中就有是否安装ESLINT，默认选择，然后是代码风格选择，默认选择 `Standard`。

这里推荐使用vscode这款编辑器，配合ESLIN插件会有很好的效果，vscode会自动标红不符合ESLINT规则的地方，同时还会做一些简单的自我修正。

要想在vscode中使用ESLINT，首先要全局按照ESLINT插件

```shell
npm install eslint -g 
```

安装完成之后，使用初始化命名生成一份配置文件

```shell
eslint --init
```

在vscode拓展面板中找到插件并搜索安装ESLINT插件

点击vscode的用户设置，配置一些简单的ESLINT设置以支持Vue单文件组件

```json
"eslint.validate": [
  "javascript",
  "javascriptreact",
  "html",
  "vue"
],
"eslint.options": {
  "plugins": [
    "html"
  ]
}
```

最后打开一个 `.vue` 单文件组件，就可以正常使用ESLINT来进行风格校验了。



## 取消ESLINT校验

如果你实在不想使用ESLIN校验，只要找到 `config/index.js` 文件。将 `useEslint:true` 设置为 `useEslint: false`即可。