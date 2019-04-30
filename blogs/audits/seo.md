# SEO
搜索引擎优化中会罗列了一些针对搜索引擎的优化点，通过这些点的优化，可以让网页在搜索结果中有一个更好的排名，从而给网站带来更多的流量。

## Document Does Not Have A Meta Description

### 简介
描述可以展示在谷歌搜索结果中。高质量的唯一的描述可以使得结果更加的和用户搜索结果相关，这可以提升搜索流量。

### 优化
 - 给每个页面的 head 中添加描述标记
 ```html
 <meta name="description" content="Put your description here." >
 ```
 - 确保每个页面都有描述
 - 给每个不同的页面不同的描述
 - 在说明中包含明确标记的事实。 描述不必是句子格式。 它们可以包含结构化数据。
 ```html
 <meta name="Description" content="Author: A.N. Author,
    Illustrator: P. Picture, Category: Books, Price: $17.99,
    Length: 784 pages">
 ```
 - 使用有价值的描述。高质量的描述会展示在搜索结果中，同时可以长期的对搜素流量带来增长。

 ## Document Doesn't Have A Title Element

 ### SEO
 搜索用户非常依赖文档标题。它是决定页面是否与搜索信息相关的重要因素。

 ## Document Doesn't Have A Valid hreflang

 ### 简介
 如果根据用户的语言和区域提供不同的内容，使用```hrefflag```链接确保搜索引擎针为该语言和地区提供正确的内容。

 ### 优化
 为 URL 每种语言版本定义一个```hreflang```链接。Lighthouse 会在报告中标记出它找到的任何不正确的```hreflang```链接。
 假设有三个版本的链接：
  - 英语版本：```https://example.com```
  - 西班牙语版本：```https://es.example.com```
  - 德语版本：```https://de.example.com```
通过给 HTML 的 head 中添加```link```元素，告诉搜索引擎这些页面都是相等的。
```html
<link rel="alternate" hreflang="en" href="https://example.com"/>
<link rel="alternate" hreflang="es" href="https://es.example.com"/>
<link rel="alternate" hreflang="de" href="https://de.example.com"/>
```
或者给 HTTP 响应的 headers 添加```link```。
```
Link: <https://example.com>; rel="alternate"; hreflang="en", <https://es.example.com>;
rel="alternate"; hreflang="es", <https://de.example.com>; rel="alternate"; hreflang="de"
```
或者是给 Sitemap 添加语言版本信息。
对于允许用户选择其语言的页面，使用```x-default```关键字。
```html
<link rel="alternate" href="https://example.com" href="x-default" />
```
每个语言的页面都应该说明所有不同语言的版本，包括它自己。
页面必须始终的相互链接。当页面 A 链接到页面 B 时，页面 B 也必须链接会页面 A。否则搜索引擎会忽略 hreflang 链接或者错误的解释它们。
```hreflang```必须使用使用语言代码。语言代码必须符合 [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 规范。```hreflang```还可以包括一个可选择的地区代码。举例说明：```en-ie```适用于给爱尔兰的英语用户,```es-ie```适用于给爱尔兰的西班牙语用户。区域代码必须遵循 [ISO 3166-1 alpha-2](https://wikipedia.org/wiki/ISO_3166-1_alpha-2) 规范。

## Document doesn't have a valid rel=canonical

### 简介
当多个页面有相似的内容时，搜索引擎认为它们是同一个页面的不同版本。举个例子，桌面和移动版本的商品页面被认为是重复的。搜素引擎会选取其中一个作为更有规范化的页面，并且会更多的爬取这个页面，而更少的爬取另一个页面。
规范化链接让我们明确的指出哪一个版本是需要爬取的。这个有多种好处：
  - 可以指定搜索结果中要展示的链接。
  - 帮助搜索引擎将多个链接合并成一个首选的链接。举例，如果其他站点给到我们站点的链接后面添加了查询参数，搜索引擎会合并这些链接到首选的版本。
  - 简化了追踪代码，追踪一个代码要比追踪多个代码更简单。
  - 它通过将指向原始内容的联合链接合并会首选网址，提高了联合内容的页面排名。
  - 它优化了爬行时间。爬取重复页面所花费的时间会浪费抓取具有真正独特内容的其他页面的时间。

### 优化
在 HTML 的 head 中添加一个 canonical 链接元素
```html
<!doctype html>
<html>
  <head>
    ...
    <link rel="canonical" href="https://example.com" />
  </head>
  ...
</html>
```
或者给 HTTP 响应 header 中添加```Link```
```
Link: https://example.com; rel=canonical
```
更多指引：
 - 确保 canonical 链接是有效的。
 - 尽可能的使用 HTTPS 的链接取代 HTTP 链接。确保页面是完全安全的，页面中没有 mixed content (HTTPS 页面加载 HTTP 资源) 错误。
 - 如果使用 hreflang 链接给不同语言和地区的用户提供不同版本的页面，确保 canonical 链接指向提供的语言和地区相应的页面。
 - 不要将 canonical 指向不同的域名，Yahoo 和 Bing 不允许这样。
 - 不要将页面指向到网站的根页面，除非它们的内容是一样的。在某些情况下可能有效，例如 AMP 或移动页面变体，但 Lighthouse 视这个方案为失败方案。

## Document Doesn't Use Legible Font Sizes

### 简介
在移动设备上字体大小小于 12px 通常是很难阅读的，并且或许需要用户手动放大到舒适的阅读大小。

### 优化
目标是页面中至少60%的文本的字体大小最小是12px。当一个页面审计失败的时候，Lighthouse 在一个4列的表格中罗列出结果。
 - Source。导致不合理文本的 CSS 规则集合的源码位置。
 - Selector。规则集合的选择器。
 - % of Page Text。规则集合影响到的文本占页面的百分比。
 - Font Size。本文最终展示的字体大小。

由于缺失 viewport 配置而导致的文本不合理。

## Document uses plugins

### 简介
插件损害 SEO 主要有两个方面：
 - 搜索引擎不能索引插件内容。插入内容不会在搜索结果中显示。
 - 许多移动设备并不支持插件，会给移动设备用户带来一个不好的体验。

### 优化
移除插件并且将内容转化成 HTML。

## Links Do Not Have Descriptive Text

### 简介
链接描述，是链接中可以点击的文本，帮助用户和搜索引擎更好的理解页面内容。

### 优化
代替通常的描述，比如下面的```click here```：
```html
<p>To see all of our basketball videos, <a href="videos.html">click here</a>.</p>
```
使用明确的描述，比如下面例子中的```basketball videos```：
```html
<p>Checkout all of our <a href="videos.html">basketball videos</a>.</p>
```
通常情况链接应该明确的指示用户，当他们点击链接之后会获得什么类型的内容。

## Page has unsuccessful HTTP status code

### 简介
搜索引擎可能无法正确的索引返回不成功的 HTTP 状态吗的页面。

### 优化
当一个页面被请求，确保服务端返回一个2xx或3xx的状态码。搜索引擎或许不会正确的索引4xx或5xx状态码的页面。

## Page Is Blocked From Indexing

### 简介
搜索引擎爬取页面是为了理解页面内容。如果没有给爬虫爬取页面的权限，它们就无法知道页面包含的内容，也就无法在搜索结果中展示页面的内容。

## robots.txt is not valid

### 简介
```robots.txt```文件告诉搜索引擎什么页面是可以爬取的。一个无效的```robots.txt```文件会导致以下两种问题：
 - 没有爬取公共的内容，导致相关页面在搜素引擎中展示的很少。
 - 爬取私有页面，在搜索结果中暴露私有信息。

## Tap targets are not sized appropriately

### 简介
点击目标是一个互动元素，比如按钮和链接，用户都是经常点击的。一个合适大小点击对象使得页面更加的移动端体验更好。
一个大小不合适的点击对象是由于尺寸太小或者太靠近其他按钮导致的。