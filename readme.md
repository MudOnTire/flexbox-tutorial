# 基本介绍

## 特点
1. flexbox是一种css display类型，提供一种更简单高效的布局方式；
2. flexbox可以对元素相对于父元素、兄弟元素进行定位、控制尺寸、控制间距；
3. flexbox对响应式有很好的支持；

## 工作原理
设置父元素的`display`属性为`flex`，则子元素都变成`flex item`，由此可以控制子元素的排列方式、尺寸、间距等；

## 兼容性
![image](https://note.youdao.com/yws/api/personal/file/WEB155d84d47df41b25711be074153bcc67?method=download&shareKey=1bfe82e9b43e8c75463829c1ce21f840)

# Flex Container

先来看一个最简单的flex示例，外层div设置`display: flex`成为一个flex container，内部的3个div则自动变为flex item：

html:
```
<div class="flex-container">
  <div class="box one"></div>
  <div class="box two"></div>
  <div class="box three"></div>
</div>
```

css:
```
.flex-container{ max-width: 960px; margin: 0 auto; display:flex; }
.box{ height: 100px; min-width: 100px; }
.one{ background: pink; }
.two{ background: lightgreen; }
.three{ background: skyblue; }
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/40edc82ed190953feb0b.png)

效果与浮动布局类似，但是如果用浮动实现的话需要写更多的代码，而flex一行就搞定了。

## 1. Justify Content

如果我们想让flex item居中排列呢，我们可以给flex container增加一个css属性：`justify-content`，它控制flex item在主轴方向（main axis，由flex-drection决定，默认为水平方向）上的对齐方式：

```
.flex-container{
  ...
  justify-content: center;
}
```

效果如图：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/721e958b368d166c0515.png)

除此之外`justify-content`还可以设置为`flex-start`, `flex-end`, `space-around`, `space-between`, `space-even`等值，具体效果请自行实验。


## 2. Align Items

实现了flex方向的居中后，垂直于主轴方向（cross axis）的居中可以用`align-items`实现。

css：

```
.flex-container{
  max-width: 960px;
  margin: 0 auto;
  display:flex;
  justify-content: center;
  height: 200px;
  background-color: white;
  align-items: center;
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/e2fb56442e15591035fa.png)

使用flex解决了以往css垂直居中实现复杂的问题！相应的，`align-items`还有`flex-start`, `flex-end`等其他值。

## 3. Flex Direction

`flex-direction`决定了主轴方向即flex item排列方向，除了默认的`row`方向之外，还可以纵向、反向（row-reverse/column-reverse）排列flex item：

css：

```
.flex-container{
  ...
  
  flex-direction: column;
  align-items: center;
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/347964e5a6af222c7490.png)


## 4. Flex Wrap

如果我们不想在窗口变窄的情况下压缩flex item，而是让超出边界的flex item换行显示那我们可以设置flex container的`flex-wrap`：

```
.flex-container{
  max-width: 960px;
  margin: 0 auto;
  display:flex;
  flex-wrap: wrap;
}

.box{
  height: 100px;
  min-width: 300px;
  flex-grow: 1;
}
```

当我们压缩窗口的时候，效果如下：
![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/82f8b8de5bf507d660ea.gif)

flex wrap还有一个值：`wrap-reverse`，设置该值后，被wrap的元素会排到其他元素上面：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/9a513626a9acfdfa46d2.gif)

由此可见，flex wrap一定程度上可以取代media query了。


## 5. Flex Row

最后，`flex-direction`和`flex-wrap`可以合并为一个属性`flex-flow`，比如：`flex-flow: row-reverse wrap`。


# Flex Item

## 1. Flex Grow

在上面所有的例子中，三个flex item只占据了flex container小部分空间，如果想让flex item占满flex container我们需要给flex item设置`flex-grow`属性。顾名思义，grow意味着增长，用于控制flex item的尺寸的伸展。

将css修改为:

```
.box { 
    height: 100px; 
    min-width: 100px; 
    flex-grow:1; 
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/bfa05a65ec26c9f9263d.png)

可以看到三个子元素平分了父元素的空间，因为此时它们的`flex-grow`都是1。如果只有一个子元素设置了`flex-grow`呢？

css：

```
.box{ height: 100px; min-width: 100px; }
.one{ background: pink; flex-grow: 1; }
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/6228e6d4bad49321032b.png)

此时two和three的大小不变，而one占据了父元素剩余空间。

如果将one的`flex-grow`改为2，而two和three改为1，我们看看会发生什么：

css:

```
.box{ height: 100px; min-width: 100px; flex-grow:1; }
.one{ background: pink; flex-grow: 2; }
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/5efa7261100e1500db7d.png)

可以看到one的宽度变成了two和three的两倍，因此flex item的尺寸和`flex-grow`的值成正比。

## 2. Flex Shrink

与`flex-grow`相对的是`flex-shrink`， `flex-shrink`用于控制子元素尺寸超过flex container后，对子元素的压缩。请看示例：
 
修改box的宽度为flex container的1/3，one、two、three的`flex-shrink`分别为1，2，3：

```
.box{ height: 100px; width: 320px; }
.one{ background: pink; flex-shrink: 1; }
.two{ background: lightgreen; flex-shrink: 2; }
.three{ background: skyblue; flex-shrink: 3; }
```

当窗口正常尺寸时，效果如下：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/904004c952387425e1b1.png)

当我们压缩窗口使其变得更窄后，效果如下：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/907b8cd34c928da195d5.png)

当flex container宽度变为540px后，子元素都被不同程度的压缩了。压缩后的one、two、three的宽度分别为250px、180px、110px，所以相比于初始宽度320px被压缩掉的宽度分别为70px、140px、210px，`70 : 140 : 210 = 1 : 2 : 3`，与flex shrink的值成反比。实际上压缩率和flex item的初始尺寸也有关系，只不过当初始尺寸一样时它带来的影响被忽略了。

假设flex shrink为`fs`，flex item的初始尺寸为`is`，flex item被压缩的尺寸为`ss`，则正确的表达式为：

```math
fs ∝ is/ss
```

## 3. Flex Basis

flex-basis用于设置flex item的初始宽/高。为什么有width和height还需要重新加一个flex-basis呢？flex-basis和width/height有如下的区别：

1. flex-basis只能用于flex-item，而width/height可以应用于其他类型的元素；

2. flex-basis和flex-direction有关，当flex-direction为row的时，flex-basis设置的是宽度，当flex-direction为column时，flex-basis设置的是高度；

3. 当flex item被绝对定位后（absolute position），flex-basis不起作用，而width/height可以；

4. flex-basis可以用于flex的简写形式，如：`flex: 1 0 200px`;

我们来看一下flex-basis的作用，将css修改如下：

```
.box{
  height: 100px;
  flex-grow: 1;
}
.one{
  background: pink;
  flex-basis: 100px;
}
.two{
  background: lightgreen;
  flex-basis: 200px;
}
.three{
  background: skyblue;
  flex-basis: 300px;
}
```

3个flex item都在原来的初始宽度基础上增加了相同的宽度：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/5eca068fe1715bd97ad3.png)

当然，这个例子如果换成使用`width`也是一样的效果，但是虽然效果一样但意义不一样，所以使用flex布局时还是应该尽量遵守规范，选合适的人去干正确的事。

## 4. Order

通过`order`属性我们可以改变flex item的排列顺序，例如：

html:

```
<section id="blocks">
  <div class="one">1</div>
  <div class="two">2</div>
  <div class="three">3</div>
  <div class="four">4</div>
</section>
```

css：

```
#blocks{
  display: flex;
  margin: 10px;
  justify-content: space-between;
}

#blocks div{
  flex: 0 0 100px;
  padding: 40px 0;
  text-align: center;
  background: #ccc;
}
```

默认排列顺序是按照flex item在html中的出现顺序：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/b15a93cf6d5f1d539664.png)

当我们修改flex item的`order`值后，flex item会按照order值升序排列：

css：

```
.one{ order: 4; }
.two{ order: 3; }
.three{ order: 2; }
.four{ order: 1; }
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/ee4f4add5b546a7916a9.png)

# 结语

flex就先简单介绍到这里，flex很强大也很简单，希望大家用的开心。