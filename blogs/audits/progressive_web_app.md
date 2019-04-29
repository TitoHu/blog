# Progressive Web App
渐进式网络应用

## Address Bar Matches Brand Colors

### 简介
将浏览器的地址栏设置成品牌的颜色可以给用户带来更加身临其境的感受。

### 优化
确保地址栏的颜色一直和我们的颜色匹配：
 - 给每个想要标记的页面的 HTML 页面添加```theme-color```的 meta 标签。
 - 给 Web App Manifest 添加```theme-color```属性。
```theme-color``` meta 标签确保了当用户作为普通网页访问我们的网站的时候浏览器地址栏打上品牌标记。给```content```设置任何有效的 CSS 值。需要将这个标签添加到任何我们想要品牌化的页面中。
```html
<head>
  <meta name="theme-color" content="#317EFB" />
  ...
```
Web App Manifest 中的```theme-color```属性确保了用户从主屏幕打开我们的应用的时候地址栏是品牌化的。和```theme-color```标签不一样，只需要在 Manifest 中定义一次。
```
{
  "theme-color": "#317EFB"
  ...
}
```

## Cache Contains start_url From Manifest

### 简介
确保 PWA 属性在离线情况下从移动设备主屏幕正确启动。

### 优化
 - 在```manifest.json```文件中定义```start_url```属性。
 - 确保 service worker 正确的缓存了和```start_url```属性匹配的资源。

## Configured For A Custom Splash Screen

### 简介
自定义启屏页让我们的 PWA 感觉像是原生的 App 应用。
当用户从桌面启动我们的 PWA 时候，Android 默认的行为是展示一个白色的屏幕直到 PWA 加载完成。用户或许会看到一个白色的空的屏幕高达200ms。使用自定义的启屏页用户将会看到我们自定义的北京颜色和应用的图标。

### 优化
只要我们在 Web App 的 Manifest 中满足以下几点情况，Android 版本的 Chrome 会自动的展示我们自定义的启屏页。
 - ```name```属性设置 PWA 的名称。
 - ```background_color```属性设置一个有效的 CSS 颜色值。
 - ```icons```数组属性指定的图片至少是 512px * 512px。
 - 图标存在并且是 PNG 格式的。

## Contains Some Content When JavaScript Is Not Available

### 简介
渐进式增强是一种网络开发策略确保了我们的站点对于大多数访问者都是可访问的。

## Content Sized Correctly for Viewport

### 简介
这项审计检查页面内容的宽度和设备宽度是否相等，当页面内容宽度大于或者小于设备宽度的时候，通常是没有针对设别做优化的情况。

### 优化
以下情况我们可以忽略这项审计：
 - 站点不需要对手机屏幕做优化。
 - 网页内容的宽度是故意大于或者小于设备宽度的。

## Has A Viewport Meta Tag With width Or initial-scale

### 简介
没有```viewport``` meta 标签，手机设备使用通用的桌面屏幕宽度渲染页面，然后把页面缩放成适合移动窗口宽度。

### 优化
在 HTML 的 head 中增加 viewport ```meta```标签
```html
<head>
  ...
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  ...
</head>
```

## Manifest Contains Icons at Least 192px

### 简介
当用户将我们的应用添加到主屏页的时候，手机设备需要一个图标来展示。这个图标就定义在 Web App Manifest 的```icons``数组属性中。
192像素的图标确保了我们的图标在最大的 Android 设备中能很好的展示。对于小一些的设备需要小一点的图标，Android 可以合理的缩放192像素的图标。换句话说，尽管我们能在更小的 Android 设备中提供更小的图标，但是这个是没有必要的。

### 优化
给 Web App Manifest 添加一个192像素的图标。
```json
{
  "icons": [{
    "src": "images/homescreen192.png",
    "sizes": "192x192",
    "type": "image/png",
  }]
}
```

## Manifest Contains background_color

### 简介
当我们的 Web 应用在主屏页面记载的时候，浏览器使用```background_color```属性来作为浏览器的颜色在应用加载的时候。

### 优化
给 Manifest 文件中添加```background_color```属性。
```json
{
  "background_color": "#red",
}
```

## Manifest Contains name

### 简介
Web App Mainfest ```name```属性是我们应用的一个可读的名称。
如果没有提供```short_name```属性，```name```属性的值将会使用在手机设备的主屏页中。

### 优化
在 Web App Manifest中添加```name```属性。
```json
{
  "name": "Hello World",
}
```
Chrome中最大长度是45个字节。

## Manifest Contains short_name

### 简介
当用户将我们的 App 添加到主屏页的时候，```short_name```是显示在主屏页中我们的 App 图标下的文本

### 优化
在 Web App Manifest 中添加```short_name```属性。
```json
{
  "short_name": "Hello World",
}
```
Chrome中最大长度是12个字节。

## Manifest Contains Start URL

### 简介
当我们的应用添加到主屏页的时候，Web App Manifest 中的```start_url```决定了当我们从主屏页启动应用的时候应用首次最先加载页面。
如果没有```start_url```属性，浏览器默认的展示当用户将应用添加到主屏页时激活的页面。

### 优化
在 Web App Manifest 中添加```start_url```属性。
```json
{
  "start_url": ".",
}
```

## Manifest Exists

### 简介
Web App Manifest是一种允许用户将 web app 添加到主屏页的技术。

## Manifest's display Property Is Set

### 简介
当应用从主屏页上加载的时候，可以在 Web App Manifest 中使用```display```属性设置应用的展示模式。

### 优化
在 Web App Manifest 中添加```display```属性，并且将值设置成以下几种值之一：```fullscreen```，```standalone```或者```browser```。
```json

```

## Page Load Is Fast Enough On Mobile

### 简介
许多用户在一种网速比较慢的情况下访问网页。确保网页能够在模拟加载其中快速的加载。

## Redirect HTTP Traffic To HTTPS

### 简介
所有的网站都应该使用 HTTPS。一旦设置了 HTTPS，就需要将所有不安全的 HTTP 请求重定向到 HTTPS 上。

## Register A Service Worker

### 简介
注册一个 Service Worker 是使用以下几种渐进式增强特性的第一步：
 - 离线模式
 - 推送通知
 - 添加到主屏页

## Response A 200 When Offline

### 简介
渐进式增强应用离线模式。如果 Lighthouse 在离线的情况下访问一个页面并且没有收到一个 HTTP 200 的响应，这个页面并不能在离线模式下访问。

### 优化
 - 给应用添加 Service Worker。
 - 使用 Service Workder 在本地缓存文件。
 - 当离线的时候，使用 Service Workder 作为网络代理，返回文件的本地缓存版本。

## User Can Be Prompted To Install The Web App

### 简介
安装 Web App 的提醒让用户将应用添加到主页面中。向主屏幕添加应用的用户将会更多的使用应用。

## Use HTTPS

### 简介
所有的网站都应用使用 HTTPS，即使网站并没有敏感的数据信息。
HTTPS 也是很多新的，功能强大的平台特性的先觉条件，比如照相或者录音。
规定中，如果一个 Web App 没有使用 HTTPS 那么就没有成为 PWA 的资格。这是因为许多主要的 PWA 技术比如 Service Workder 都需要HTTPS。