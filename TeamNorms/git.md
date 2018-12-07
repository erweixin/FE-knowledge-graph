# git 团队规范

## commit 格式规范

规范的格式能够为 commit 带来更好的可读性，并且能够提供更多的历史信息，方便快速浏览和快速查找信息。这里推荐目前使用最广的 [angulr 规范](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)。

```js
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
该规范由 `type`、`body` 和 `footer` 3部分组成：

### header
header 部分由 `type` (必需）、`scope` (可选) 和 `subject` (必需) 3部分组成。

`header: <type>(<scope>): <subject>`

#### Type
`Type` 用于说明 commit 的类别，只允许使用下面8个标识。
- build: 影响构建系统或外部依赖项的更改（示例范围：gulp，broccoli，npm）
- ci: CI 配置文件和脚本的更改 (示例范围: Travis,Circle, BrowserStack, SauceLabs)
- docs: 仅更改文档
- feat: 新功能
- fix: 修复bug
- perf: 提升性能的代码更改
- refactor: 没有增加新功能或者修补bug的代码更改
- style: 不影响代码含义的更改 (空格、格式、缺少分号等)
- test: 添加缺失的测试用例或更正现有测试用例

#### Scope
用于说明 commit 影响的范围，视项目不同而不同。

#### Subject
`Subject` 是 commit 目的的简短描述，不超过50个字符。
- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号（.）

### Body
`Body` 部分是对本次 commit 的详细描述，可以分成多行。

### Footer
`Footer` 用于包含 Breaking Changes 的信息，也用于关闭 github issue。


## git 分支规范

## git 合并规范