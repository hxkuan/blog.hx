---
title: Koa(v2.5.0)源代码阅读笔记
categories: 笔记
tags:
  - 后端
  - 编程
  - node
  - npm
  - 基础
date: 2018-05-08 16:07:28
---
从源码上来看，koa确实比express精简了很多。主体的koa包只有application，context，request，response这4个文件，更让我惊喜的是，application整个就是ES6的class。这种亲切感，几乎瞬间就平衡了koa和express在我心中的位置。
<!-- more-->
从application这个文件的构造函数来看，有proxy，middleware，subdomainOffset，env，context，request ，response 7个属性；
以整个application文件来说，proxy，env，subdomainOffset只是全局的设置属性；
context，request，response则只是用来以此为模板，分别初始化了ctx的context，request，response对象（因此app.context是不等于ctx.context，从下文可以看出来，所有的用户都是共用一个app对象的，但每个用户都有自己的ctx对象）；
而只有middleware才具有具体的操作，这里也就可以明白，为什么说Koa完全是由中间件(middleware)来处理请求，当然application是继承Event类的，也就是说可以通过Event来处理。
```js
this.proxy = false;
this.middleware = [];
this.subdomainOffset = 2;
this.env = process.env.NODE_ENV || 'development';
this.context = Object.create(context);
this.request = Object.create(request);
this.response = Object.create(response);
```

整个application文件，其实只有listen()，toJSON()，inspect()，use()，callback()，handleRequest()，createContext()，onerror() 8个public方法，以及一个private方法respond()，而inspect还是toJSON的alias,也就是说，其实就只有7个public方法了。

如果说，那两个方法是Koa最主要的方法，我想那无疑是use()和listen()了，这两个函数一个是用于添加middleware，一个则是用于创建一个server。从代码上来看，use就三步操作，
第一步是判断传入的是否是个function，这里可以看出来，middleware本质其实就是个处理函数，因此也不需要因为高大上的命名而感觉到深奥；
第二步则是将传入的middleware generators处理，不得不说我不喜欢generators，而且事实上也证明，以后少用generators，`Support for generators will be removed in v3.`,这是源代码中的提示，也就是说v3版本将不在支持generators了，可喜可乐；
第三步就是将传入的middleware存入到app的middleware数组中，(这里就有个坑，按理说app的应该是middlewares，好吧，查了有道词典，原来根本就没有这个词，估计middleware单复数形式是一样的。)

对于listen()其实就更简单了，就一步：使用Node http模块创建一个server并listen()。这里要提的是，传入server的回调函数就是app的callback()方法，也就是说，所有的操作其实都是以callback()开始的，从这方面来说，listen()方法只是简化了创建server的流程而比不是唯一的选择，实际上app中并没有https server创建的接口，也就是说Koa官方也是默认用户绕过listen()方法直接使用http/https的createServer()来创建server。


```js
callback() {
  const fn = compose(this.middleware);
  if (!this.listeners('error').length) this.on('error', this.onerror);
  const handleRequest = (req, res) => {
    const ctx = this.createContext(req, res);
    return this.handleRequest(ctx, fn);
  };
  return handleRequest;
}
```
这是整个callback()的代码，其实也简单，
首先将app的middleware转换为一个执行方法，内部其实就是一个递归函数，在这里可以看出来，上一个middleware中的next其实就是下一个middleware，以此类推，如此形成了“洋葱”结构的回调。(忍不住吐槽一下，js的弱类型很大程度上简单了编码，但作为框架型的编码，弱类型真的不是很实用，至少时不时的得判断一下参数的类型，就够麻烦，不判断又会出大问题)；
然后，就是查看是否有error事件，没有就注册一个默认的。Koa中默认必有的事件只有error，这个通过app.eventNames()就能查看详情；
最后使用app的createContext()初始化ctx后，就交给了app的handleRequest()处理了。下面讲讲createContext()和app的handleRequest()。

```js
const context = Object.create(this.context);
const request = context.request = Object.create(this.request);
const response = context.response = Object.create(this.response);
context.app = request.app = response.app = this;
context.req = request.req = response.req = req;
context.res = request.res = response.res = res;
request.ctx = response.ctx = context;
request.response = response;
response.request = request;
```
这是createContext()的部分代码，一段很有特色的代码。在这，我们可以看出koa的3个对象是真正的做到了你中有我我中有你了，也就是说只要有一个对象，就能从中取得其他的两个，这种无脑的解决方式真的开了眼睛，但不得不说，使用的时候肯定会很方便。

对于handleRequest(),值的一提的是，其内部调用了两个方法，一个是onFinished()，它对访问的socket进行了处理，从而增加了访问的稳定性；而另一个respond()，则是调用res的end()方法来完结请求，这也算是一种不同吧。这边需要注意的是，如果使用res的end()方法来中止请求，事实上是不会终止middleware后续操作的，也就是说respond()或者onerror()还是会被执行，从这方面可以看出，Koa是不建议我们使用Node的end()接口，否则这点优化还是需要的。
而到这里，真个app或者说是真个Koa算是结束了。

整体下来，收获还是很大的；从Express到Koa，从Koa v1到v2，也算是从复杂到简单，再到如此简单，不得不说还是挺让人感慨的。而更让我感慨的是对于class的使用.
从以往的感受中，阅读源码大部分不适应来源于多变的语法结构。比如function中套function，多种多样的js类封装。毕竟对于业务同学来说复杂的函数其实并不常用，虽说，能静下心来研究的人才是高手，但像这种简简单单就能降低学习门槛的事为什么要抵制呢？
最后，分享一句话：细节决定成败，这也是我看完Koa后最有感触的一句话。



