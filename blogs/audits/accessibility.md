# Accessibility
无障碍

## Buttons Have An Accessible Name

### 简介
没有名称的按钮对于依赖屏幕阅读器的用来说是没有办法使用的。当一个按钮没有名称的时候，屏幕阅读器会显示“button”。

### 优化
Lighthouse 标记出了每一个没有通过这项审计的元素，修复方案取决于每个元素被标记的类型：
对于```<button>```元素和有```role="button"```的元素：
 - 设置元素的内部文本。
 - 设置```aria-label```属性。
 - 设置```aria-labelledby```属性设置为具有屏幕阅读器可见的文本的元素。换句话讲，你所指的元素不应该有```display: none```的样式或者是```aria-hidden="true"```在 HTML 中。
对于```<input type="button">```的元素：
 - 设置```value```属性。
 - 设置```aria-label```属性。
 - 设置```aria-labelledby```属性。和上面一点一样。
对于```<input type="submit">```和```<input type="reset">```元素：
 - 设置```value```属性或者删除它。浏览器会给“submit”或者“reset”默认值，当```value```属性被删除的时候。
 - 设置```aria-label```属性。
 - 设置```aria-labelledby```属性。和上面一点一样。


## Document Doesn't have A Title Element

### 无障碍
文档标题提供了这个页面用途的概述。标题对于依赖屏幕阅读器的用户来说是非常重要的，因为它们对于页面的上下文较少。

### 优化
确保每个页面都有标题
```html
<!doctype html>
<html>
  <head>
    <title>Create Good Title</title>
  </head>
  ...
</html>
```
如果有一个很大的站点，使用 [HTML Improvements Report](https://support.google.com/webmasters/answer/9073702?visit_id=636921162652494635-1815449588&rd=1) 来爬这个站点然后发觉任何缺失标题的页面。
创建一个好标题的提示：
 - 确保它们具有描述性和简洁性。避免使用像```Home```那样模糊的描述。
 - 避免关键词堆积。这些对用户没有用，并且搜索引擎会将这个页面标记为垃圾页面。
 - 避免重复或样板标题。
 - 品牌标题是可以的，但是要简明扼要。
在 [Create descriptive page titles](https://support.google.com/webmasters/answer/35624) 中获取更多的提示。

## Every Form Element Has A Label

### 简介
标签阐明了表单元素的用途。尽管每个元素的目的对于视力无障碍的用户来说非常明显，但是对于使用屏幕阅读器的用户来讲却并非如此。
更多信息，参见[Create Amazing Forms](https://developers.google.com/web/fundamentals/design-and-ux/input/forms/#label_and_name_inputs_properly)

### 优化
有四种方式可以将每个表单元素和标签关联起来：
潜在的标签：
```<label>First Name <input type="text"/></label>```
明确的标签：
```<label for="first">First Name <input type="text" id="first"/></label>```
```aria-label```：
```<button class="hamburger-menu" aria-label="menu">...</button>```
```aria-labelledby```：
```html
<span id="foo">Select seat:</span>
<custom-dropdown aria-labelledby="foo"></custom-dropdown>
```

## Every Image Has An alt Attribute

### 简介
信息性的图像应该有```alt```属性，该属性包含了该图像文本内容的描述。屏幕阅读器使视力不好的用户将文本内容转换为可以使用的表单来使用站点。屏幕阅读器并不能转换图片。所以图片包含了一些重要的信息，这些信息对于视觉不好的用户来说就是不可访问的。

## No Element Has a tabindex Attribute Greater Than 0

### 简介
```tabindex```属性使得元素键盘可导航。正值表示元素的显示导航顺序。

### 优化
对于不应该键盘导航的元素，设置它们的```tabindex```值为```-1```或者是```0```。如果需要元素在 tab 的顺序中出现的较早，可以考虑在 DOM 中向前移动。