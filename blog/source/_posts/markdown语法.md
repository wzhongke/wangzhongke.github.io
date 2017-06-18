---
title: markdown语法
date: 2017-06-17 19:42:25
tags: [markdown]
categories: nodejs
---

## 斜体 
*这是斜体1*
_这是斜体2_
```
*这是斜体1*
_这是斜体2_
```
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
<span style="text-decoration:underline">我带下划线</span>
<span style="text-decoration:overline">我带上划线</span>

## 超链接
```
[text](https://www.sogou.com)
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
```
![Alt text][1]

```
[1]: https://thumbnail0.baidupcs.com/thumbnail/c96194bc7ae2c597e673997dc9966ae0?fid=2318483978-250528-428917790560367&time=1497769200&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-5OYOIKsv2Z%2FWavAZFWBRxaGsrfs%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=3910915653000159178&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video "optional title"
```

[1]: https://thumbnail0.baidupcs.com/thumbnail/c96194bc7ae2c597e673997dc9966ae0?fid=2318483978-250528-428917790560367&time=1497769200&rt=sh&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-5OYOIKsv2Z%2FWavAZFWBRxaGsrfs%3D&expires=8h&chkv=0&chkbd=0&chkpc=&dp-logid=3910915653000159178&dp-callid=0&size=c710_u400&quality=100&vuk=-&ft=video "optional title"