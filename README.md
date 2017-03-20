# vue
基于vue2.0
构建

vue有自己的脚手架构建工具vue-cli,使用起来非常方便，使用webpack来集成各种开发便捷工具，比如：

代码热更新，修改代码之后网页无刷新改变，对前端开发来说非常的方便
PostCss，再也不用去管兼容性的问题了，只针对chrome写css代码，会自动编译生成支持多款浏览器的css代码
Eslint，统一代码风格，规避低级错误，对于有代码洁癖的人来说是绝对的好东西，不过有些地方的代码校验有时候也挺麻烦的-.-
bable，ES2015出来已经有一段时间了，但是不少浏览器还没有兼容ES6.有了bable，放心使用ES6语法，它会自动转义成ES5语法。
Stylus，类似于SASS/SCSS，但是可以不写{}和“：”，使用起来还是很方便的
...
除此之外，vue-cli已经使用node配置了一套本地服务器和安装命令等，本地运行和打包只需要一个命令就可以搞定，非常的方便

vue2点官方推荐http请求工具变为axios
v-el标签废弃 改为了ref
广播变为了$emit
还有许多改变的地方请自行对照文档
> vue

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

