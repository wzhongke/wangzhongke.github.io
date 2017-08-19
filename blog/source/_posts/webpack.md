


```sh
## 初始化 npm
npm init
## 安装 webpack
npm install webpack --save-dev

## 打包某个文件
webpack hello.js hello.bundle.js

npm install css-loader style-loader --save-dev

webpack hello.js hello.bundle.js --module-bind 'css=style-loader!css-loader!'

```

```js
// 对css的执行
require('style-loader!css-loader!style.css)

```

```

```