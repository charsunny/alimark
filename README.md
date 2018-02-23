# alimark -  支付宝小程序Markdown渲染库

[![Travis](https://img.shields.io/travis/TooBug/alimark/master.svg)](https://travis-ci.org/charsunny/alimark)
![MIT](https://img.shields.io/badge/LICENSE-MIT-green.svg)

![alimark](./screenshot.png)

## 背景和功能

用于在支付宝小程序中渲染Markdown文本。

众所周知，Markdown的渲染一般需要解析成HTML来渲染。然而小程序并没有提供HTML渲染的功能，甚至连利用JS动态创建UI元素都不可能。因此所有的Markdown渲染库全部无法在小程序下正常工作。

本库可以实现在小程序下渲染Markdown内容，支持图片、表格在内的大部分Markdown特性。

## 使用方式

### 1. 准备工作：

1. 下载并拷贝`alimark`目录到小程序根目录
2. ACSS中引用样式：`@import '../alimark/alimark.acss';`
3. AXML中引用结构：`<import src="../alimark/alimark.axml"/>`
4. JS中引用`alimark`：`const alimark = require('../alimark/alimark.js')`

### 2. JS部分

首先需要确定一个用于`alimark`渲染使用的数据名称（默认情况下为`alimark`），然后在页面的`data`中指定：

```javascript
page({
	data:{
		// 确定一个数据名称
		alimark:{}
	}
});
```

接下来在`Page`的生命周期函数（如`onReady`）中，调用`alimark.parse(md, this, options)`即可。

具体的参数说明：

- `md`，必填，需要渲染的Markdown字符串
- `this`，必填，`Page`实例
- `options`，其它的参数
	- ~~`imageWidth`，图片的宽度，如包含图片，则为必填，以`px`为单位~~ 新版小程序图片可以自适应宽高
	- `name`，对应上面指定的数据名称，默认为`alimark`

### 3. AXML部分

```html
// data中的参数和上方确定的数据名称保持一致
<template is="alimark" data="{{...alimark}}"></template>
```

### 实例

```javascript
// 引入alimark
var alimark = require('../alimark/alimark');
// 需要渲染的Markdown文本
var md = '# hello, world\n\nI love you, alimark!';


Page({
	data: {
		// 确定一个数据名称
		alimark:{}
	},
	onLoad() {
		alimark.parse(md, this, {
			// 新版小程序可自适应宽高
			// imageWidth: wx.getSystemInfoSync().windowWidth - 40,
			name: 'alimark'
		})
	}
});
```

## 特性

- 标题
- 段落
- 代码段
- 引用
- 无序列表
- 有序列表
- 粗体
- 强调（斜体）
- 删除线
- 行内代码
- 图片
- 表格
- HTML标记
	- 视频

## 测试

```shell
npm install
npm test
```

## 开源协议

MIT

如果你也使用了 alimark，欢迎提 PR 将自己的小程序加入列表

## 版本记录

### v1.2.1 2017-07-24

- 支持使用`video[poster]`属性添加视频封面图 #15 by @kilik52

### v1.2.0 2017-06-30

- 一些渲染细节样式修正 #9 #10
- 图片渲染使用`widthFix`模式，不再动态计算高度 #11 #12
### v1.1.0 2017-01-22

- 添加删除线渲染（`~~deleted~~`）
- 添加HTML视频解析（需求&初始实现 by @littledu）
- 解析功能补齐自动测试

### v1.0.0 2016-12-12

- 基本功能
