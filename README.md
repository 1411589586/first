# SSR服务端渲染
服务端渲染简单理解是将组件或页面通过服务器生成html字符串，再发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序。
## 服务端渲染特性（SSR）
### 1. seo
不同爬虫工作原理类似，只会爬取源码，不会执行网站的任何脚本（Google除外，据说Googlebot可以运行javaScript）。使用了React或者其它MVVM框架之后，页面大多数DOM元素都是在客户端根据js动态生成，可供爬虫抓取分析的内容大大减少(如图一)。另外，浏览器爬虫不会等待我们的数据完成之后再去抓取我们的页面数据。服务端渲染返回给客户端的是已经获取了异步数据并执行JavaScript脚本的最终HTML，网络爬中就可以抓取到完整页面的信息。

### 2. 首屏加载
首屏的渲染是node发送过来的html字符串，并不依赖于js文件了，这就会使用户更快的看到页面的内容。尤其是针对大型单页应用，打包后文件体积比较大，普通客户端渲染加载所有所需文件时间较长，首页就会有一个很长的白屏等待时间。

``` javascript
	<a href="#case2">test2</a>
	<a href="#case3">test3</a>
	<a href="#case4">test4</a>
```

### 数据的获取

``` javascript
import React from 'react'

export default class extends React.Component {
 static async getInitialProps({ req }) {
   const userAgent = req ? req.headers['user-agent'] : navigator.userAgent
   return { userAgent }
 }

 render() {
   return (
     <div>
       Hello World {this.props.userAgent}
     </div>
   )
 }
}
```