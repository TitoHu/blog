# Performance 性能
Performance 介绍了和网页性能相关的指标和技术点，通过这些技术点和指标，我们可以对网页进行优化，从而给用户带来更好的体验。

## Critical Request Chain 关键请求链

### 简介
Critical Request Chain 是 [Critical Rendering Path][CRP](CRP/关键渲染路径)优化策略中的一个概念。CRP 能够让浏览器确定加载哪个资源和确定资源加载的顺序，从而更快的加载一个页面。

### 优化
通过 Lighthouse，会生成一个类似以下内容的图表：
```bash
Initial navigation
|---lighthouse/ (developers.google.com)
    |---/css (fonts.googleapis.com) - 1058.34ms, 72.80KB
    |---css/devsite-googler-buttons.css (developers.google.com) - 1147.25ms, 70.77KB
    |---jsi18n/ (developers.google.com) - 1155.12ms, 71.20KB
    |---css/devsite-google-blue.css (developers.google.com) - 2034.57ms, 85.83KB
    |---2.2.0/jquery.min.js (ajax.googleapis.com) - 2699.55ms, 99.92KB
    |---contributors/kaycebasques.jpg (developers.google.com) - 2841.54ms, 84.74KB
    |---MC30SXJEli4/photo.jpg (lh3.googleusercontent.com) - 3200.39ms, 73.59KB
```
以上内容就是页面的 Critical Request Chain。```lighthouse/```到```/css```是一条链，```lighthouse/```到```css/devsite-googler-buttons.css```是另外一条链。这个图表中同样展示了每个资源的大小和加载消耗的时间。我们可以通过这个图表优化 CRP：
 - 减少关键资源的数量：移除，延迟下载，异步加载或者其他方式。
 - 减少关键资源的大小，降低资源的下载时间。
 - 优化加载剩余关键资源的顺序：尽快的下载所有关键资源从而减少关键路径长度。

## Defer unused CSS 延迟未使用的 CSS

### 简介
在页面中通常使用```link```来添加样式文件：
```html
<!doctype html>
<html>
  <head>
    <link href="main.css" rel="stylesheet">
```
由于```main.css```文件和 HTML 是分开存储的，所以它是被浏览器作为外部样式文件下载。
默认情况下，浏览器在它展示、渲染任何内容到用户屏幕之前，必须下载、解析并且执行所有它遇到的外部样式。在这些样式被执行之前，浏览器并不会展示内容，因为这些样式或许会影响页面的样式。
每一个样式文件必须从网络上下载下来。这些额外的网络加载时间会明显的增加用户看到内容之前需要等待的时间。
无用的 css 内容同样会减缓浏览器构建```render tree```。```render tree```类似```DOM tree```，除此之外，它还包含了每个节点的样式。为了构建```render tree```，浏览器必须访问所有的```DOM tree```，同时判断每个节点使用了哪些 CSS 规则。所以无用的 css 越多，判断 css 规则所消耗的时间就会越多。

### 优化
```Critical CSS```是指页面展示所需要的 CSS。通常情况下，页面的加载应该只被```critical CSS```阻塞。
理论上讲，最好的方式是将```critical CSS```以内联的方式插入到 HTML 文件的```head```中。一旦 HTML 文件下载下来之后，浏览器就有了显示这个页面所需要的所有内容，不需要任何的其他网络请求。
```html
<!doctype html>
<html>
  <head>
    <style>
    /* Critical CSS here */
    </style>
```
```Uncritical CSS```是指页面或许后期需要使用的 CSS。比如，假如点击一个按钮会弹出一个模态窗口，而且这个模态窗口只会在按钮点击后弹出。那么这个模态窗口所需要的 CSS 就是```uncritical CSS```。因为这个模态窗口永远不会在页面第一次加载时展示。

#### 检测 Critical CSS
Chrome Devtools 的 Coverage 可以帮助我们发现 Critical CSS 和 Uncritical CSS。具体内容见 [View used and unused CSS with the Coverage tab](https://developers.google.com/web/tools/chrome-devtools/css/reference#coverage)

#### 内联 Critical CSS
我们可以使用一些工具来帮助我们自动的内联 Critical CSS，比如 Webpack 可以使用 [isomorphic-style-loader](https://github.com/kriasoft/isomorphic-style-loader/)。

#### 延迟 Uncritical CSS
同样也有一些工具可以帮助我们延迟 Uncritical CSS，比如 [loadCSS](https://github.com/filamentgroup/loadCSS)。

## Enable Text Compression 开启文本压缩

### 简介
文本压缩减小了网络响应包含的文本内容的字节大小，下载的字节越少意味着加载的速度越快。

### 优化
Lighthouse 可以显示出哪些资源没有被压缩。
当浏览器请求资源的时候，它会在请求的 header 的```accept-encoding```中罗列出它支持的所有的压缩方式。所有的现代浏览器都会如此。服务器使用浏览器支持的其中的一种格式来编码响应，并且会在响应的 header 的```content-encoding```中指出使用了哪一种格式。
检查 server 是否压缩了响应
 - 进入 Network 控制板
 - 点击一个返回了响应的请求
 - 点击 Headers 标签
 - 检查 Response Header 项中的```content-encoding```

## Estimated Input Latency 预计输入延迟

### 简介
输入反应是用户察觉我们应用性能的关键因素。应用有 100ms 来响应用户输入，任何超出这个时间的都会让用户感觉到应用的迟缓。[更多信息见 Measure Performance with the RAIL Model](https://developers.google.com/web/fundamentals/performance/rail)。

### 优化
为了让应用更快的响应用户，需要优化代码在浏览器中运行的方式。更多细节的优化点见[Rendering Performance](https://developers.google.com/web/fundamentals/performance/rendering/)。

## First Contentful Paint 首次内容绘制

### 简介
First Contentful Paint(FCP)计量了从开始导航到浏览器渲染第一个 DOM 内容的 bit 的时间。这对于用户来说是一个重要的里程碑，应为它表示页面已经实际加载了。

### 优化
加速 FCP，可以加速资源文件的下载速度或者减少阻断浏览器渲染 DOM 内容的工作。
 - 减小阻拦渲染的外部样式的大小以及页面依赖的脚本。
 - 使用 HTTP 缓存来提升资源重复请求的速度。
 - 减小和压缩文本资源的大小加快资源的下载。
 - 通过 Tree Shaking 或者是代码拆分优化 JavaScript 启动并且减少 JavaScript 有效负载，最终的目标是在页面加载的时候减少 JavaScript 的运行。

## First CPU Idle 首次 CPU 空闲

### 简介
First CPU Idle 是指页面首次可以交互的时候：
 - 大多数情况下，界面中的 UI 元素可以交互的时候。
 - 页面在合理的时间内平均响应大多数用户输入。

### 优化
有两个通用的策略来提升优化时间：
 - 减少在页面渲染前必须要下载或者执行的必须或者关键资源的数量。
 - 减小每个关键资源的大小。

## First Meaningful Paint 首次有效绘制

### 简介
页面加载是一个用户如何感受到页面性能的关键部分。这个审计内容定义了用户接触页面的主要内容是可见的时间。

### 优化
First Meaningful Paint 的得分越低，页面展示它主要内容就越快。[Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/) 是实现加速 First Meaningful Paint 的特别有用的方式。

## Avoid Enormous Network Payloads 避免大量的网络负载

### 简介
减少网络请求的总量可以加快页面加载时间并且节省用户花费在流量上的钱。

### 优化
以下策略可以减小负载大小：
 - 延迟加载直到用户需要的时候
 - 优化请求，越小越好，比如：
   - 开启文本压缩
   - 压缩 HTML、JS 和 CSS。
   - 使用 WebP 取代 JPEG 和 PNG。
   - 设置 JEPG 的压缩比例到85。
 - 缓存请求，这样重复的请求就不用重复下载资源。

## Has Multiple Page Redirect 页面多次重定向

### 简介
重定向降低了页面加载速度。
当浏览器请求的的资源被重定向了，服务器通常会返回一个如下的 HTTP 响应：
```
HTTP/1.1 301 Moved Permanently
Location: /path/to/new/location
```
浏览器必须重新发起一个其他 HTTP 请求到这个新的地址去获取资源。这个附加的网络流程会减缓好几百毫秒的资源资源加载时间。

### 优化
Lighthouse 中列出了被重定向的资源。更新这些资源地址。这些地址应该是当前资源的地址。尤其是要避免关键渲染路径上的资源重定向。
如果是使用重定向来将手机用户定向到手机端，可以考虑使用响应式的设计。

## JavaScript Bootup Time Is Too High 

### 简介
这项审计衡量了 JavaScript 对页面加载性能造成的总体影响。JavaScript 可以在多种方面降低页面速度：
 - 网络消耗。内容越大意味着下载时间越长。
 - 解析和编译消耗。JavaScript 在主线程中解析和编译。当主线程繁忙的时候，页面就不能响应用户的输入。
 - 执行消耗。JavaScript 同样在主线程中执行。如果页面在它实际需要之前运行了大量的代码，同样会延缓页面响应时间。这个是用户体验页面速度中非常重要的。
 - 内存消耗。如果 JavaScript 中有大量的引用，会明显的消耗大量的内存。当消耗大量的内存的时候页面会变得很卡很慢。内存泄漏会导致页面完全卡死。

### 优化
以下几点方式可以优化：
 - 仅传输用户需要的代码。
 - 压缩代码。
 - 减小代码。
 - 移除无用的代码。
 - 缓存代码。

## Keep Server Response Times Low

### 简介
用户不喜欢页面加载时间太长，服务器响应太慢也是页面加载时间太长的一个重要因素。

### 优化
提升服务器响应的第一步是找到服务器返回内容必须要做的任务，然后计算出这些任务需要花费的时间，一旦我们找到了非常消耗时间的任务，想办法优化就可以了。

## Minify CSS

### 简介
减小 CSS 可以提升页面加载性能。
CSS 文件通常要比它们应该的大小要大，比如：
```css
h1 {
  background-color: #000000;
}
h2 {
  background-color: #000000;
}
```
可以减小成：
```css
h1, h2 {
  background-color: #000000;
}
```
在浏览器的视角上，这两个代码在功能上是一样的，但是第二种占用的字节更小。压缩器甚至可以移除空格来进一步的减小大小。
```css
h1,h2{background-color:#000000;}
```
一些压缩器还会使用一些小技巧来压缩资源大小，比如```#000000```可以使用```#000```来代替。

### 优化
使用 CSS 压缩器来压缩 CSS 代码：
 - 对于一个不是经常更新的小项目，可以使用在线的压缩工具来压缩文件。
 - 对于专业的开发来讲，很可能想要设置一个自动的工作流在发布代码前自动的压缩代码

## Offscreen Images

### 简介
由于用户在页面加载后看不到屏幕外的图片，所以没有必要在页面初始化加载中去下载这些屏幕外的图片资源。换句话说，延迟加载屏幕外图片，可以提升页面加载时间和页面可以交互的时间。

### 优化
可以使用图片懒加载技术来解决这个问题。

## Optimize Images

### 简介
优化图片可以更快的加载同时节省流量。

### 优化
Lighthouse 中可以看到哪些图片是需要优化的，有几种方式优化：
 - 使用其他图片格式。比如 SVG。SVG 图片是矢量图，由于是使用文本方式存储的，所以拉升和缩放并不会对图片大小造成影响，同时也可以压缩文本的大小，
 - 提供图片的多种格式。给一个图片提供多种格式，比如针对不同分辨率的屏幕提供不同大小的图片。
 - 使用 Web Services 和 CDNs。可以使用 Web Service 和 CDN 提供的一些方法来优化图片。

## Oversized Images

### 简介
理想情况下，服务器提供的图片大小不应该比在用户浏览器中显示的图片大，任何大于的图片都意味着浪费资源并且降低页面加载速度。

### 优化
Lighthouse 列出了每个审计失败的图片，提供了如何处理这些图片的方式。
 - 处理这种图片最主要的策略叫做“响应式图片”。使用响应式图片，给每个图片生成多种版本，并且使用媒体查询，设备大小等方式在 HTML 或者 CSS 中标示使用什么版本的图片。
 - 另一种策略是使用矢量图，比如 SVG。在优先大小的代码中，SVG 图片可以缩放成任何大小。
 - 还有一种策略叫做“Client hint”。当浏览器请求图片的时候会带上浏览器的视窗大小和像素比，然后服务端根据这些数据返回图片。然而这个特性只有有限的几个浏览器支持。

## Speed Index

### 简介
数值越低越好。

### 优化
减小 Speed Index 得分，需要优化页面来在视觉上加载的更快：
 - [Optimizing Content Efficiency](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/)。
 - [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)。

## Preload Key Requests

### 简介
预加载请求可以让页面加载更快。
假设页面的关键请求链如下：
```
index.html
|--app.js
   |--styles.css
   |--ui.js
```
```index.html```文件中申明了语句```<script src="app.js"></script>```。当```app.js```执行的时候，会调用```fetch()```去下载```style.css```和```ui.js```。页面并不会显现直到这两个剩余的资源下载、解析和执行。

### 优化
在 HTML 中定义预加载链接，来告诉浏览器下载这些资源越快越好。
```html
<head>
  ...
  <link rel="preload" href="style.css" as="style">
  <link rel="preload" href="ui.js" as="script">
  ...
</head>
```

## Render-Blocking Resources

### 简介
快速的页面加载能够提升页面访问量，增加转化率。
可以通过内联首屏需要的样式和脚本，延迟加载哪些不需要的，来提升页面加载速度。

### 优化
Lighthouse 会罗列出监测到的阻塞渲染的链接或者脚本，目标就是减少这些数量。
Lighthouse 标记了三种阻塞渲染链接：脚本，样式和 HTML imports。如果优化取决于我们使用的资源类型。
 - 对于关键的脚本，可以考虑将它们内联到 HTML 中。对于不关键的脚本，考虑使用```async```或者```defer```属性标记它们。
 - 对于样式，考虑将它们分成多个文件，通过媒体查询组织，然后给每个链接添加```media```属性。当页面加载的时候，浏览器只会在获取匹配用户设备的样式文件时候阻塞首次渲染。
 - 对于不关键的 HTML imports，用```async```属性标记它们。作为一个通用规则```async```属性和 HTML imports 一起使用的越多越好。

## Serve Images in Next-Gen Formats

### 简介
JPEG 2000，JPEG XR 和 WebP 这些图片格式比对应的老版本的 JPEG 和 PNG 格式有更好的压缩比和图片质量。将图片编码成这些新格式意味着图片会加载的更快，并且消耗更少的流量。
WebP 在 Chrome 和 Opera 浏览器中支持并且在 web 中给图片提供了更好的有损和无损压缩方式。

### 优化
并不是所有浏览器都支持 WebP，但是类似的节省方式在大多数主流浏览器上都有一个可替代的下一代图片格式。我们需要给其他浏览器提供一个降级策略。

## Time to Interactive

### 简介
Time to Interative (TTL) 计时衡量了一个页面需要多久才能交互。交互的定义如下：
 - 页面展示了有用的内容，也就是 First Contentful Paint。
 - 大多数可见的页面元素注册了事件处理器。
 - 页面在 50ms 内响应的用户操作。

### 优化
为了提升 TTI 得分，延迟或者去除页面加载中无用的 JavaScript 工作。

## User Timing Marks and Measures

### 简介
User Timing Api 允许我们统计应用中 JavaScript 的性能。基本的原理是我们决定脚本中的哪个部分需要优化，然后使用 User Timing Api 处理脚本的这些部分。这时候，我们可以通过这个 Api 获取到结果，或者在 Chrome DevTools Timeline Recording 中查看结果。

### 优化
这个审计并不是一中“通过”或“失败”。
更多内容见 [User Timing Api](https://www.html5rocks.com/en/tutorials/webperformance/usertiming/)。

## Use An Excessive DOM Size

### 简介
一个大型的 DOM 树会损害页面的性能：
  - 网络效率和加载性能。如果服务端发送了一个大型的 DOM 树，那么会发送大量无用的字节。这也会延缓页面加载时间，因为浏览器或许要解析大量的节点。
  - 运行时性能。当页面中用户和脚本交互的时候，浏览器需要重新计算节点的位置和样式，一个大型的 DOM 树以及样式会严重的影响到页面的渲染。
  - 内存性能。当我们通常的查询一个节点的时候```document.querySelectorAll('li')```，会在毫不知情的情况下引用到一个大型的节点，会占用设备大量的内存。

### 优化
最佳的 DOM 树：
 - 总共有少于1500个节点。
 - 最大深度为32的节点。
 - 没有拥有超过60个子节点的父元素。
通常情况下，寻找一种当我们需要的时候创建节点，当我们不需要的时候删除节点的方式。

## Uses inefficient cache policy on static assets

### 简介
HTTP 缓存可以加速页面加载时间当重复访问的时候。
当浏览器请求一个资源，服务端提供的资源可以告诉浏览器这个资源应该被暂时的存储或者缓存多长时间。之后任何关于这个资源的请求，浏览器都会使用它的本地备份来替代从网络中再次获取它。

### 优化
配置服务端返回的 HTTP 响应头```Cache-Control```
```
Cache-Control: max-age=31536000
```
```max-age```指令告诉浏览器资源需要缓存多少秒。```31536000```等于1年：60秒 * 60分钟 * 24小时 * 365天 = 31536000秒
如果资源更改和新鲜度很重要，则使用```no-cache```，但仍然希望缓存带来的一些速度优势。浏览器会缓存这些设置```no-cache```的资源，但是首先会检查服务端确保这些资源仍然是最新的。
有很多中指令来自定义浏览器缓存不同的资源。
 - [Defining optimal Cache-Control policy](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#defining_optimal_cache-control_policy)
 - [Cache-Control specification](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
 - [Cache-Control (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)
Chrome从内存缓存中提供最多请求的资源，速度很快，但是当浏览器关闭的时候会被清空

[CRP](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)