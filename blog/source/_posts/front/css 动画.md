---
title: css 动画
---


# transitions

可以通过设置 transitions 来实现过渡效果。其用法如下：
```CSS
transition:  [ <transition-property> |
               <transition-duration> |
               <transition-timing-function> |
               <transition-delay> ]
```
{% raw %}
<style>.wrapper{position:relative;border:1px #aaa solid;width:500px;height:500px;margin:0 auto 10px;padding:10px}.shadow{-webkit-box-shadow:5px 5px 5px #aaa;-moz-box-shadow:5px 5px 5px #aaa;box-shadow:5px 5px 5px #aaa;margin-bottom:10px}.normal,.example2,.example3{width:100px;height:100px;position:absolute;top:210px;left:210px;border-radius:50px;background-color:red;text-align:center;transition:all 1s ease-in-out}.example2{background-color:blue;transition-property:top,left;transition-duration:1s,1s;transition-delay:0s,1s}.example3{background-color:purple;transition-property:top,left,border-radius,background-color;transition-duration:2s,1s,0.5s,0.5s;transition-delay:0s,0.5s,1s,1.5s}.wrapper:hover .normal{top:0;left:0}.wrapper:hover .example2{top:398px;left:398px}.wrapper:hover .example3{top:0;left:398px;border-radius:0}.wrapper p{line-height:70px;color:white;font-weight:bold;margin-left:0 0 10px}</style><div class="wrapper shadow"><div class="normal shadow"><p>Normal</p></div><div class="example2"><p>Example2</p></div><div class="example3"><p>Example3</p></div></div>
{% endraw %}


# transforms
可以通过 transforms 来实现变形效果。目前有2D变形和3D变形，不过3D变形只有新的浏览器中支持。其语法如下:
```CSS
transform: none|transform-functions
```
它的属性值如下表

值    | 含义      
:-----|:-----
none  |  定义不进行转换。    
matrix(n,n,n,n,n,n) | 定义 2D 转换，使用六个值的矩阵。  
matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n) |  定义 3D 转换，使用 16 个值的 4x4 矩阵。  
translate(x,y) | 定义 2D 转换。   
translate3d(x,y,z) | 定义 3D 转换。   
translateX(x)  | 定义转换，只是用 X 轴的值。 
translateY(y)  | 定义转换，只是用 Y 轴的值。 
translateZ(z)  | 定义 3D 转换，只是用 Z 轴的值。 
scale(x,y) | 定义 2D 缩放转换。 
scale3d(x,y,z) |  定义 3D 缩放转换。 
scaleX(x) |  通过设置 X 轴的值来定义缩放转换。  
scaleY(y)  | 通过设置 Y 轴的值来定义缩放转换。  
scaleZ(z)  | 通过设置 Z 轴的值来定义 3D 缩放转换。  
rotate(angle) |  定义 2D 旋转，在参数中规定角度。  
rotate3d(x,y,z,angle) |  定义 3D 旋转。   
rotateX(angle) | 定义沿着 X 轴的 3D 旋转。    
rotateY(angle) | 定义沿着 Y 轴的 3D 旋转。    
rotateZ(angle) | 定义沿着 Z 轴的 3D 旋转。    
skew(x-angle,y-angle) |  定义沿着 X 和 Y 轴的 2D 倾斜转换。  
skewX(angle)   | 定义沿着 X 轴的 2D 倾斜转换。  
skewY(angle)   | 定义沿着 Y 轴的 2D 倾斜转换。  
perspective(n) | 为 3D 转换元素定义透视视图

可以将 transform 同 transition 结合使用
