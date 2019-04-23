+++
date = "2018-04-05T21:49:57-07:00"
title = "Angular-xss"

+++

## Angular-xss防护

> 为了系统性的防范 XSS 问题，Angular 默认把所有值都当做不可信任的。 当值从模板中以属性（Property）、DOM 元素属性（Attribte)、CSS 类绑定或插值表达式等途径插入到 DOM 中的时候， Angular 将对这些值进行无害化处理（Sanitize），对不可信的值进行编码。
> Angular 定义了四个安全环境 - HTML，样式，URL，和资源 URL：

* HTML：值需要被解释为 HTML 时使用，比如当绑定到 innerHTML 时。
* 样式：值需要作为 CSS 绑定到 style 属性时使用。
* URL：值需要被用作 URL 属性时使用，比如 `<a href>`。
* 资源 URL的值需要作为代码进行加载并执行，比如 `<script src>` 中的 URL。

Angular 会对前三项中种不可信的值进行无害化处理，但不能对第四种资源 URL 进行无害化，因为它们可能包含任何代码。在开发模式下， 如果在进行无害化处理时需要被迫改变一个值，Angular 就会在控制台上输出一个警告。

## example

``` html
<h3>Binding innerHTML</h3>
<p>Bound value:</p>
<p class="e2e-inner-html-interpolated">{{htmlSnippet}}</p>
<p>Result of binding to innerHTML:</p>
<p class="e2e-inner-html-bound" [innerHTML]="htmlSnippet"></p>
```
```js
export class InnerHtmlBindingComponent {
  // For example, a user/attacker-controlled value from a URL.
  htmlSnippet = 'Template <script>alert("0wned")</script> <b>Syntax</b>';
}
```
![](https://angular.cn/generated/images/guide/security/binding-inner-html.png)

[Angular - 安全](https://angular.cn/guide/security#offline-template-compiler)

> 目前存在的逃逸姿势

[AngularJS沙盒逃逸姿势总结 - Seaii's Blog](https://seaii-blog.com/index.php/2017/09/02/68.html)

![](https://image.3001.net/images/20190410/15548669847528.png)

## JSON数据过滤

OWASP json过滤一遍
[GitHub - OWASP/json-sanitizer: Given JSON-like content, The JSON Sanitizer converts it to valid JSON.](https://github.com/owasp/json-sanitizer)


```json
            theString = theString.Replace(">", "&gt;");  
            theString = theString.Replace("<", "&lt;");  
            theString = theString.Replace(" ", "&nbsp;");  
            theString = theString.Replace("\"", "&quot;");  
            theString = theString.Replace("\'", "&#39;");  
            theString = theString.Replace("\\", "\\\\");//对斜线的转义  
            theString = theString.Replace("\n", "\\n");  //注意php中替换的时候只能用双引号"\n"
            theString = theString.Replace("\r", "\\r");  
```