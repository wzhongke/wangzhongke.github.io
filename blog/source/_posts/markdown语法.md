---
title: markdown语法
date: 2017-06-17 19:42:25
tags: [markdown]
categories: nodejs
---

## 斜体
```
*这是斜体1*
_这是斜体2_
```
*这是斜体1*
_这是斜体2_
<!-- more -->
## 粗体
```
**这是粗体**
__这是粗体__
```
**这是粗体**
__这是粗体__

## 带删除线
```md
~~删除线.~~
```
~~删除线.~~

## 下划线
```
<span style="text-decoration:underline">我带下划线</span>
<span style="text-decoration:overline">我带上划线</span>
```
## 超链接
```
[text](https://www.sogou.com)
<https://www.sogo.com>
```
[text](https://www.sogou.com)
<https://www.sogo.com>
## 引用
引用的区块也可以使用其他的Markdown语法
```
> 这是一个引用
> > 这是第二级引用
```
> 这是一个引用
> > 这是第二级引用
## 锚点
[问内链接](#user-content-斜体);

## 列表
### 无序列表
```
- Red
- Green
- Blue
```
- Red
- Green
- Blue
### 有序列表
```
1. 1
2. 2
3. 3
```
1. 1
2. 2
3. 3
### 代办列表
```
- [ ] 不勾选
- [x] 勾选 (为aiks)
```
- [ ] 不勾选
- [x] 勾选 (为aiks)
## 代码
\`\`\`
\`\`\`
```java
new String();
```
## 表格

```
First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right

```

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right

## 分割线
```
---
***
```
---
***

## 图片
```
![Alt text][1]
[1]: https://www.sogou.com/index/images/logo_440x140.v.1.png "搜狗搜索 optional title"

![Alt text](https://www.sogou.com/index/images/logo_440x140.v.1.png)
```
![Alt text][1]

[1]: https://www.sogou.com/index/images/logo_440x140.v.1.png "搜狗搜索 optional title"

![Alt text](https://www.sogou.com/index/images/logo_440x140.v.1.png)