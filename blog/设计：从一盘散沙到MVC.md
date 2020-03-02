​       node.js框架的发展历程本身就是从散装一路向MVC的思想发展的过程，这篇搁置了很久的原因是因为在部署过程中碰到的问题让我一直考虑是否该换一个框架改写一下。这个问题具体我会在部署成功之后写到部署篇里。本篇主要想讨论的是我从使用express框架一路更改代码结构的辛酸历程。 

一、MVC到底意味着什么，和我们用的MVVM框架思想不同在哪里？ 

MVC编程思想适用于目前很多编程框架，它代表着从程序核心到业务逻辑处理到视图显示效果的分层管理机制，在后台框架和代码中的体现就是将数据库相关的业务（例如CRUD操作等等）集中在一部分管理，将用户视图集中在一部分管理，将业务中数据和控制业务流转的逻辑集中在一部分处理。 

我们现今使用的基于MVVM原理的框架多由MVC的思想发展而来，它在控制核心业务和数据管理的基础上对视图展示部分做了细分，降低了原来MVC思想中视图层中数据和HTML骨架间的耦合度。 

二、如何将服务器代码按MVC模式分割 

首先明确我使用的express框架的基本目录格式。 

![mvc-1](https://github.com/solerji/Learning-Summary/blob/master/pictures/blog/mvc2.png)
原工程的标配目录是bin,public,views,routes。分别用于配置管理，公共资源，视图主体，路由。可以看出它虽然做了部分区分但耦合度仍然很高，解耦的关键在于将数据库相关的核心服务拆分，将用户处理拆分，由于本工程的前端是单独存放于一处，部署使用nginx做转发操作，所以在页面方面的处理我们暂时不做讲解。 

起初，笔者将所有逻辑代码放置于routes，最终结果是代码较长，且有些操作引用比较冗余，在参考别人代码的基础上决定将代码中的数据库查询操作单独封装，写入service文件夹中。大概形成如下格式代码。即查询之后将Promise返回Controller性质的文件中统一处理。 

```
module.exports.getArticleList = function() { 



let sql = 'SELECT aid, title, show_content FROM article' 



  return new Promise((resolve, reject) => { 



  connection.query(sql, (err, rows) => { 



    if (err) { 



      reject(err) 

    } else { 



      resolve(rows) 

    } 

   }) 

  }) 

} 
```

例如： 

```
router.post('/api/tagsAndTime', async (req, res) => { 



try { 



let tagAndTime = await tagService.getTagTimeLine(req.body.tagName) 



// console.log('[获取标签及时间轴成功！]') 

res.status(200).send({ 

list: tagAndTime 



}) 

} catch (err) { 



res.status(500).send('服务器端错误!', err.message) 



} 

}) 
```

使用async形式代码有两种形式的捕获错误方式，开始使用的是if(result){}else{}的模式，后来发现try…catch的捕获机制要比自己控制error方便些，于是全部换了try…catch。 

这个代码基本是把数据CRUD和查询结果分解了出来，当时觉得代码就整顿好找了很多，但没有意识到在此基础上路由拆分出去的好处。直到最近将后台换成了egg.js框架。 
![mvc-2](https://github.com/solerji/Learning-Summary/blob/master/pictures/blog/mvc1.png)

Egg自己就把controller和路由分开管理。在使用过程中不断感觉到真香，就是可以对自己路由的管理做到一目了然，这种情况更有利于接口的统一管理，比如你要对get的头文件做处理的时候，真的要好操作许多。但就是找路由的对应方法的时候还得换文件找，实在有点麻烦。 

三、MVC形式的代码维护起来到底有没有更容易 

√ 

1.错误的定位速度会更快一些 

某些凭经验判断的错误可以直接找到对应位置直接查错，在代码混杂时需要排除一些干扰项。 

2.利于集中处理 

比如错误处理，路由管理等等，在改动一些管理性质的文件确实更加方便一些，横向管理会更方便。 

× 

找方法时跳转文件费劲了些。 
