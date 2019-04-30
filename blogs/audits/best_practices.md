# Best Practices

## Avoids Application Cache

### 简介
Application Cache 和 AppleCache 一样已经废弃了。

### 优化
考虑使用 Service Worker 缓存 Api 来代替。

## Avoids console.time() In Its Own Scripts

### 简介
如果正在使用```console.time()```来计算页面的性能，可以考虑使用 User Timing Api 来代替。

## Avoids Date.now() In Its Own Script

### 简介
如果正在使用```Date.now()```来计算时间，考虑使用```performance.now()```来替代。```performance.now()```提供了一个更高的时间戳解决方案，并且始终于系统时钟无关的恒定速率增加，系统时钟可以手动调整或者歪斜。

### 优化
在 Lighthouse 中罗列的页面中每个使用 Date.now() 的地方，将它们都替换成```performance.now()```。
更多信息参见 [performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) API。

## Avoids Deprecated APIs

### 简介
废弃的 APIs 在某一个时刻会被从 Chrome 中移除。在这些 API 被移除之后调用它们会导致程序报错。

### 优化
Lighthouse 在报告中标记了使用到的废弃的 APIs。更多信息见 [Chrome Platform Status](https://www.chromestatus.com/features#deprecated)。

## Avoids document.write()

### 简介
对于网络环境较慢的用户来说，通过```document.write()```动态插入的外部脚本会延缓主页内容的显示几十秒。
更多信息见 [Intervening against document.write()](https://developers.google.com/web/updates/2016/08/removing-document-write)。

### 优化
在报告中，Lighthouse 罗列出了每个调用```document.write()```的地方。

## Avoids Mutation Events In Its Own Script

### 简介
以下这些 DOM 的变异事件会损害新能，并且已经废弃了：
 - ```DOMAttrModified```
 - ```DOMAttributeNameChanged```
 - ```DOMCharacterDataModified```
 - ```DOMElementNameChanged```
 - ```DOMNodeInserted```
 - ```DOMNodeInsertedIntoDocument```
 - ```DOMNodeRemoved```
 - ```DOMNodeRemovedFromDocument```
 - ```DOMSubtreeModified```

## Avoid Old CSS Flexbox

### 简介
老版本的 Flexbox 已经废弃了并且比最新版本的要慢。

### 优化
Lighthouse 罗列出了在样式文件中的每一个```display: box```。把它们替换成新的语法```display: flex```。

## Avoids Requesting The Geolocation Permission On Page Load

### 简介
用户对页面加载时自动请求获取其位置权限的页面会感到不信任或者是疑惑。可以当用户点击类似“查看附近商店”的时候请求获取用户的位置权限。请确保这些动作在获取用户位置的时候清楚并且明确的表达了意图。

## Avoids Requesting The Notification Permission On Page Load

### 简介
一个好的通知应该是及时的、相关的和明确的。如果一个页面在加载的时候就想用户请求通知权限，这些通知内容或许和用户并不相关。

## Display Images With Incorrect Aspect Ratio

### 简介
如果一个图片展示出来的比例和图片原本的比例不同，那么这个图片看起来或许是变形的，很可能给用户带来一个不愉快的用户体验。

### 优化
 - 避免给一个可变大小的容器设置元素的宽度或者高度为百分比。
 - 避免设置与图片尺寸大小明确不同的高度或者是宽度的值。
 - 考虑使用 [css-aspect-ratio](https://www.npmjs.com/package/css-aspect-ratio) 或 [Aspect Ratio Boxes](https://css-tricks.com/aspect-ratio-boxes/) 来保护比例。
 - 如果可以的话，在 HTML 中设置图片的宽度和高度会是一个很好的尝试，这样浏览器就可以分配图片的空间，避免了页面加载之后图片跳出来的现象。但是对于响应式的图片来说是很困难的，除非直到视窗的尺寸，否则很难知道图片的大小。

## Includes Front-End JavaScript Libraries With Known Security Vulnerabilities

### 简介
入侵者有自动爬取扫描网站的工具，它们可以扫描网站中的漏洞。当爬虫扫描到网站上的漏洞的时候，会告诉入侵者。这时候，入侵者需要考虑的就是如何利用这些漏洞了。

### 优化
停止使用 Lighthouse 标记出来的库。如果这个库已经更新的新的版本来修复这个漏洞，那么就使用新的库，否则替换一个新的库。

## Manifest's short_name Won't Be Truncated When Displayed on Homescreen

### 简介
当用户将 Web App 添加到主屏页的时候，```short_name```属性将会作为文本展示在 app 图标的下面。如果字符长度超过了12，那么就会被截断。
如果没有设置```short_name```属性，如果```name```属性足够短，Chrome 会退回使用该属性。

### 优化
确保 Web App Manifest 文件中的```short_name```属性少于12个字节。
```json
{
  "short_name": "Hello world",
}
```
如果在 Manifest 中没有设置```short_name```属性，确保```name```属性少于12个字节

## Links to cross-origin destinations are unsafe

### 简介
当使用```target="_blank"```来打开其他页面的时候，其他页面或许和你的页面运行在同一个线程中，除非启用了站点隔离。如果其他页面运行了大量的脚本，那么你的页面也会遭到性能上的损失。

### 安全
其他页面可以通过```window.opener```来获取页面的```window```对象，这就提供了一种攻击方式。因为其他页面可以悄悄将你的页面重定向到一个恶意的连接上。更多信息见 [About rel=noopener](https://mathiasbynens.github.io/rel-noopener/)。

### 优化
给所有 Lighthouse 报告标记出来的链接添加```rel="noopener"```或者```rel="noreferrer"```。通常情况下使用```target="_blank"```的时候总是添加```rel="noopener"```或者```rel="noreferrer"```。
```html
<a href="https://example.domain.com" taret="_blank" rel="noopener">
  Example Web Site
</a>
```

## Prevents Users From Pasting Into Password Fields

### 简介
一些网站声称不允许用户粘贴密码可以提高安全性。在 [Let Them Paste Passwords](https://www.ncsc.gov.uk/blog-post/let-them-paste-passwords) 中表示这种说法是毫无根据的。
密码粘贴提升安全性是因为它允许用户使用密码管理器。密码管理器通常给用户生成一个很健壮的密码，并且安全的存储这些密码，同时当用户需要使用密码登录的时候自动的粘贴密码到密码输入区域中。

### 优化
移除哪些阻止用户粘贴密码密码区域的代码。
```javascript
let input = document.querySelector('input');
input.addEventListener('paste', (e) => {
  e.preventDefault(); // 这会阻止粘贴
});
```

## Some Insecure Resources Can Be Upgraded To HTTPS

### 简介
当一个安全的（HTTPS）站点请求一个不安全（HTTP）的资源，这叫做 [mixed content](https://developers.google.com/web/fundamentals/security/prevent-mixed-content/what-is-mixed-content) 错误。有些浏览器会默认的阻止加载不安全的资源。如果页面依赖这些不安全的资源，那么当资源被阻拦的时候这些页面很可能不能正常的工作。

## Uses HTTP/2 For Its Own Resources

### 简介
HTTP/2 可以更快的处理资源数据，并且通过网络传输的数据更少。

## Uses Passive Event Listeners to Improve Scrolling Performance

### 简介
在触摸和滚动事件监听器上设置```passive```选项可以提升滚动的性能。
更多概述见 [Improving Scrolling Performance with Passive Event Listeners](https://developers.google.com/web/updates/2016/06/passive-event-listeners)。

### 优化
给所有 Lighthouse 标记出来的事件监听器添加```passive```属性。通常情况下，添加```passive```标记给每个不会调用```preventDefault()```的```wheel```、```mousewheel```、```toucestart```、```touchmove```事件监听器。
在浏览器中设置```passive```属性是很简单的
```javascript
document.addEventListener('touchstart', onTouchStart, { passive: true });
```
然而在那些不支持 passive 事件监听的浏览器上，第三个参数是一个布尔值，表示事件是否应该冒泡或捕获。所以，使用上述的语法会造成非计划中的问题。
在 polyfill in [Feature Detection](https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md#feature-detection) 中学习如果安全的实现 passive 事件监听器。
