​      在工作过程中遇到了关于css布局的问题，是在Tab内容中处理好两栏布局，以及在此布局基础上继续进行多复选框排版。在排版过程中对所用到和遇到可以选型的布局进行总结。由此将CSS相关布局及适用的范围整理成此文，以便查阅。

**一、默认情况的布局**

display: block

块级元素按文档中书写出现的顺序放置，垂直放置且每个块级元素在上一个元素下面另起一行

dispaly: inline

行内元素在空间内总是在同一行，空间不够的话溢出文本会移到新的一行

**二、可选择使用的原生布局方式**

**1.浮动布局**

**例：float + margin + BFC 三栏**

BFC是什么？（块格式化上下文）

它有6大规则：

（1）盒子按照垂直方向一个一个的放置。

（2）盒子的垂直方向的间距是根据margin来决定的，相邻的盒子margin会发生重叠。

（3）子盒子会跟父盒子的左边相接触。

（4）BFC区域不会和float box发生重叠

（5）作为一个隔离的独立容器，容器的子元素不会影响外面的布局。

（6）计算宽高时，浮动元素也参与计算。

如何生成一个BFC?

（1）float的值不为none；

（2）overflow的值不为visible；

（3）display的值为inline-block table-cell table-caption；

（4）position的值为absolute或fixed

由BFC引出CSS布局中所有的FC(格式化上下文，一个渲染区域，决定了其子元素如何定位以及其他元素之间的关系和相互作用)，比较常用的有BFC/IFC（行内格式化上下文）/GFC（网格布局格式化上下文）/FFC（自适应格式化上下文）。这几个FC就是我们可以在CSS3中使用的一些元素的基础。

**2.弹性盒子（基于FFC）**

![img](https://pic1.zhimg.com/v2-f47d8c085407dfadaa3ef28b1084b788_b.png)



这图看起来很复杂，官方解释是说这个布局有两根轴线，是一维布局。在使用前我并没有读懂这个所谓“一维布局”的说法，但在处理2行3列元素的布局模式时，我突然明白，这个布局之所以被称之为一维布局的原因是，它在处理行间间距时并没有一个很好的方式。所以采取弹性盒子解决“单行多列”的问题效率会比较高，而“多行多列”的布局更适合使用表格布局和网格布局。

**3.网格布局（基于GFC）**

![img](https://pic4.zhimg.com/v2-2ad78628e2abe3361c419849fd16a18f_b.png)

网格布局在某种程度上是表格布局的升级版，它也是以固定小区域框住元素的形式使元素整齐的布局方式，但不同于表格的处理方法是，在网格布局提供的属性中，可以更为完整的找到调整行间关系的属性。且在多个大小相同的元素布局方式上代码量更少，实现效果更好。

网格布局这边遇到一个第一行元素位置与此后几行不一样的bug,参考了同事的解决方案，调整元素与元素间间距。

**4.表格布局**

把视图分割成表格布局，虽然也可以解决一些布局问题，但是局限性很强。优点在于支持表格样式的浏览器都可以兼容。

**5.流式布局**

又叫百分比布局，该布局的特点是宽度用百分比来设计，高度或图片大小等写死。这样的做法在适应相近分辨率的屏幕上有优势，但如果屏幕分辨率相差较大，该布局的某些元素就会乱掉。

**6.静态布局**

最原始的布局方式，不再多说。

**三、经典的CSS布局样式解决方案**

**多列布局**

**1.两栏布局（左侧固定，右侧自适应）**

```
<template>
<div class="checkbox">
<div class="head">头部栏</div>
<div class="left">左边栏</div>
<div class="right">右边栏里到底有什么</div>
<div class="footer">底部栏</div>
</div>
</template>


<style scoped>
.head{
background-color:wheat;
width: 1000px;
height: 100px;
}
.footer{
background-color:wheat;
width: 1000px;
height: 100px;
margin-top: 200px;
}
.left{
margin-left: 200px;
position: relative;
float: left;
background-color: aqua;
width: 300px;
height: 200px;
}
.right{
position: relative;
float: left;
background-color:violet;
height: 200px;
}
.checkbox{
width: 1500px;
height:1000px;
}
</style>
```

![img](https://pic2.zhimg.com/v2-31c5bd5f8e6f1514c9ab43518b2fac11_b.png)



**2.三栏布局（两侧固定，中间自适应）**

**（1）圣杯布局**

```
<div class="checkbox">
<div class="head">头部栏</div>
<div class="contain">
// 最顶部一定是中间栏
<div class="center">中间栏</div>
<div class="left">左边栏</div>
<div class="right">右边栏</div>
</div>
<div class="footer">底部栏</div>
</div>
</template>


<style scoped>
.head{
background-color:wheat;
height: 100px;
}
.footer{
background-color:wheat;
height: 100px;
clear: both;
}
.left{
position: relative;
margin-left: -100%;
float: left;
right: 300px;
background-color: aqua;
width: 300px;
height: 200px;
}
.center{
float: left;
background-color:salmon;
width: 100%;
}
.right{
float: left;
margin-right: -300px;
background-color:violet;
width: 300px;
height: 200px;
}
.contain{
padding-left:300px;
padding-right:300px;
}
</style>
```

![img](https://pic4.zhimg.com/v2-b44eb92a99842669daa5ff27c3e7038f_b.png)



**（2）双飞翼布局（没有用到 relative属性，最小页面宽度小于圣杯布局）**

```
<template>
<div class="checkbox">
<div class="head">头部栏</div>
<div class="contain">
<div class="center">中间栏</div>
</div>
<div class="left">左边栏</div>
<div class="right">右边栏</div>
<div class="footer">底部栏</div>
</div>
</template>


<style scoped>
.head{
background-color:wheat;
height: 100px;
}
.footer{
background-color:wheat;
height: 100px;
clear: both;
}
.left{
float: left;
background-color: aqua;
width: 300px;
margin-left: -100%;
height: 200px;
}
.center{
margin-left:300px;
margin-right:300px;
background-color:salmon;
}
.right{
float: left;
margin-left: -300px;
background-color:violet;
width: 300px;
height: 200px;
}
.contain{
float: left;
width: 100%;
}
```

![img](https://pic3.zhimg.com/v2-0244856cd75e1983d589391dd9a1d1e2_b.png)



**垂直居中布局**

**垂直居中布局实现方法**

**（1）line-height（父元素高度确定）**

缺点是文字不能换行，换行布局会乱

```
<template>
<div class="checkbox">
<p>hello night</p>
</div>
</template>




<script>
export default {
name: 'vue',
data () {
return {
}
}
}
</script>
<style scoped>
.checkbox{
background-color:red;
height: 100px;
width: 100px;
}
p{
line-height:100px;
}
</style>
```

**（2）vertival-align（元素高度不确定）**

```
<template>
<div class="checkbox">
<div class="test">
文字垂直
在中间
</div>
<div class="winter"></div>
</div>
</template>
<style scoped>
.test{
display: inline-block;
background-color:blue;
vertical-align: middle;




}
.winter{
display: inline-block;
background-color:skyblue;
height:100%;
vertical-align: middle;
}
```

![img](https://pic1.zhimg.com/v2-b1597e1a5be4f2da9dc242c2d43331b4_b.png)



**（3）table-cell和table（目前只实现了垂直居中，水平居中还在考虑怎么写，应该要指定列位置才能完成）**

```
<template>
<div class="checkbox">
<div class="test">
lalalalllalala
</div>
<!-- <div class="winter"></div> -->
</div>
</template>
<style scoped>
.checkbox{
display: table;
width: 500px;
height:500px;
background-color: bisque;
}
.test{
display: table-cell;
vertical-align:middle;
background-color: brown;
}
</style>
```



![img](https://pic4.zhimg.com/v2-16163e405172517a11995460d95d9d0b_b.png)

**（4）absolute定位top50%+transform（未知块元素高度）**

```
<template>
<div class="checkbox">
<div class="test">
</div>
<!-- <div class="winter"></div> -->
</div>
</template>


<style scoped>
.checkbox{


}
.test{
position: absolute;
margin:auto;
top: 50%;
left:50%;
transform: translate(-50%,-50%);
width: 50px;
height:1000px;//块大小更改时会不断改动位置
background-color: brown;
}
</style>
```

**（5）cacl()计算居中（这种方式也可以随视图移动而移动）**

```
<template>
<div class="checkbox">
<div class="test">
lalalalllalala
</div>
</div>
</template>


<style scoped>
.test{
width: 100px;
height:100px;
position: absolute;
left: calc(50% - 50px);
top:calc(50% - 50px);
background-color: brown;
}
</style>
```

![img](https://pic2.zhimg.com/v2-54ea3295138e2cfec9b929d569bf9abd_b.png)

**（6）绝对定位（已知块元素高度）**

```
<template>
<div class="checkbox">
<div class="test">
</div>
</div>
</template>
<style scoped>
.checkbox{
}
// 第一种，较为准确
.test{
position: absolute;
margin:auto;
top: 0;
bottom:0;
left:0;
right:0;
height:100px;
width: 50px;
background-color: brown;
}
// 用这样的可能不会完全准确，取决于margin-left的值
.test{
position: absolute;
margin:auto;
top: 50%;
left:50%;
right:0;
margin-left: -10px;
height:100px;
width: 50px;
background-color: brown;
}
</style>
```

![img](https://pic1.zhimg.com/v2-fed41d6bb0905d1ac4efece294115364_b.png)