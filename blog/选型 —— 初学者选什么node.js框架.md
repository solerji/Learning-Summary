在博客开始搭建时，笔者在粗略学习node.js原理后，决定选用一个框架完成博客后台的构建。在github上和各大平台上的综合分析之后锁定了较为稳定(老旧）的Express框架，和基于Koa2作为初学的备选项（别问我为啥不选egg.js，我只是想先了解一下不好用的再看看好用的）。为什么最终选择Express而没有选择Koa2,这两个框架初学者怎么样取舍是该文章讨论的重点。 

一、Express和Koa2在语法上的不同 

Express沿用ES5的标准，而Koa2的写法中使用了ES6,7的编码格式。这应当是它们两个最显而易见的区别。如果使用async/await较少的朋友可以先选用Express练手，熟悉后逐步过渡到ES2017语法，然后再使用Koa2。(该代码示例处是作者在使用express后出现回调地狱情况后，在该框架中加入了async写法，对比Koa2封装好的来讲有些相似，但性能上远不如koa2。) 

（代码样式示例） 

Express: 

```javascript
router.post('/api/tagsAndTime', async (req, res) => { 
try { 

  let tagAndTime = await tagService.getTagTimeLine(req.body.tagName) 

  res.status(200).send({ 

  list: tagAndTime 

  }) 

} catch (err) { 

  res.status(500).send('服务器端错误!', err.message) 

  } 

}) 


```



Koa2:  

```javascript
router.get('/signout', async(ctx, next) => { 
  ctx.session = null; 
  console.log('登出成功') 
  ctx.body = true 
}) 
```

二、Express 和 Koa2的中间件机制不同 

​      Express框架的特点是大而全，它具有路由和模板等功能，它的中间件是一个接一个执行的，response响应通常是在最后一个中间件中实现。（反正在最初使用它的时候我遇到了大量的callback回调，如果各位打算入手千万要提前想好怎么管理callback，不然回调的次序和回调的次数最后会搞糊你。） 

​      Koa库把express大而全的功能进行模块化封装，开发者可以只选用其核心模块开发。Koa库的思路为洋葱模型，其核心原理就是**ctx（context）上下文的保存和传递（替代express的req和res），中间件的管理和next方法的实现**。中间件之间通过next函数联系，一个中间件调用next()后，会将控制权交给下一个中间件，直到不再执行next()就沿原路线返回，控制权交由前一个中间件。（这个next()是一个promise，相对来讲比较好处理。） 

​     总结而言，想体验回调到promise的神奇之旅，可以选express，其他情况，还是选koa吧。 

三、注意 

​      补充一条，想部署服务器直接用的新手最好不要选express，这个框架运行在服务器上的时候真的需要一个好的维护。 

四、怎么选择 

​      笔者保持一种观点，在学习时循序渐进了解每个框架的发展历程，如果手头有选择，且有时间的时候，应当从最旧的框架尝试学习。这样可以很好的了解该框架的缺点，然后再使用成熟框架迁移。如果只是为了快速完成工程或者快速上手，应从最新的入手逐步回退。 