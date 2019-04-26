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

[CRP](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)