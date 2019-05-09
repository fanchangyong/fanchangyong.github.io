---
layout: post
title: 介绍Next.js
date: 2019-05-09
---

## 学习Next.js之(一) - Next.js的优势
 * 很早就听说过这个项目，最近花了半天时间大概看了一下，感觉挺有兴趣。但是因为学习的时间比较短，并且并没有在实际项目中使用的经验，可能以下描述存在错误的地方，请各位同学以官方文档为准。

根据Next.js官网的介绍，它是一个React Framework.那么为什么我们需要这么一个Framework呢？那我们首先来说一下单纯使用React做项目有哪些痛点:
   - React缺少路由的功能。我们知道，做单页应用前端路由是必不可少的一项功能，而React本身并不提供，一般需要引入一个第三方的库`react-router`来解决。
   - 配置过于复杂。我们知道，使用现代的前端框架如（Vue、React）等编写的代码，浏览器是不能直接运行的，通常都需要使用Webpack等打包工具进行打包。而Webpack的配置是相当复杂的。如果你尝试过全部手工配置，就会知道想写出来一个Hello World可能都是很难的一件事。
	- SEO的问题。现代的单页应用由于在HTML结构中只返回一个script标签，而HTML本身其实是空的，是靠后续JS代码的执行去渲染出整个网页。而大多数搜索引擎都是不支持JS的。也就是说，在搜索引擎看来，你的网页其实是个空白页面。这当然对SEO是很不利的。
	- 首屏渲染速度的问题。对于单页应用，首屏渲染的流程大致如下：下载HTML代码->下载JS代码->执行JS代码->完成渲染。而对于普通的Server端渲染的网页而言，只需要: 下载HTML代码->完成渲染。速度的差距是很明显的。
	- 代码体积过大的问题。一个单页应用的JS代码通常是打包到一个文件中的。而随着项目越来越大，整个JS文件的体积会越来越大。我们需要一种机制，就是一段代码(比如一个module或组件)只有当它真正被用到的时候才去执行加载。
	- 项目结构混乱。React作为一个底层框架，它本身对项目的目录结构是没有任何要求和看法的，这本身是无可厚非的。但在实际项目中，我们总是需要一个团队规范，这样才容易维护和协作。Next.js在这方面规定了一些规范，比如pages下放的组件都是对应到一个Route的，而且文件名本身就是Route的路径。其他子组件需要放到components中。不管你是否认可这种方式，我觉得有一个规范还是好的。另外我是比较认可它的这种组件的分配方式的。
	
Next.js就是为了解决以上这些问题而生的，我们来看一下它是如何一一解决以上问题的：
   - Next.js内置了Routing的功能，并且根据官网的介绍：`All subsequent routes get lazy-loaded, for scalability sake.` 这一点还是比较适合做大型程序的。
   - Next.js是零配置的，但你也可以修改它内置的配置选项。
   - Next.js支持SSR(Server Side Rendering)，这样一下解决了SEO和首屏渲染速度两大问题。
   - Next.js支持自动化的code split，根据官网：`Every import you declare gets bundled and served with each page. That means pages never load unnecessary code!`
	 
另外，Next.js还支持：
  - 内置了`styled-jsx`，来提供`isolated scoped CSS`的写法，这也是一种CSS-in-JS的解决方案
  - 内置了`Populating <head>`的功能，使用React写的JSX代码，都是被渲染到页面的<body>中的。而如果想要往<head>里加东西，就需要使用`react-helmet`这样的库。Next.js内置了这项功能
  - 简化的生命周期函数。我们在生命周期函数中最常做的一件事情，就是`componentDidMount`和`componentDidUpdate`的时候去服务端取数据，然后放到redux中。Next.js通过`getInitialProps` 简化了这一过程。`getInitialProps`只能用在`pages`组件中。

好了，我们今天就先介绍到这里。Next.js的东西还是挺多的，我们下期继续~