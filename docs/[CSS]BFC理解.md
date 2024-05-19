> https://blog.csdn.net/jiangqing993/article/details/125321816

-----

> BFC 是一个独立的布局环境,可以理解为一个容器,在这个容器中按照一定规则进行物品摆放,并且不会影响其它环境中的物品

### 触发条件

- 根元素或包含根元素的元素
- 浮动元素float＝left|right或inherit（≠none）
- 绝对定位元素position＝absolute或fixed
- display＝inline-block|flex|inline-flex|table-cell或table-caption
- overflow＝hidden|auto或scroll(≠visible)

### BFC可以解决的问题

- 清除浮动
- margin重叠
- 自适应两列布局

#### 1、解决外边距塌陷的问题

1）、父子元素的外边距塌陷问题
我们在一个元素内部子元素，设置了一个margin-top:20px; 结果父元素有一个具体顶部20px的距离，明显不符合我们的预期

```
* {
    margin: 0;
    padding: 0;
}
.box {
    width: 400px;
    height: 400px;
    background-color: skyblue;
}

.box1 {
    width: 100px;
    height: 100px;
    background-color: red;
    margin-top: 30px;
}


<div class="box">
    <div class="box1"></div>
</div>
```

box距离顶部有一定的距离
如果我们要解决父子元素边距塌陷问题，我们给box加上 overflow:hidden;属性

2)、解决第二种兄弟元素之间外边距塌陷
两个兄弟元素设置margin:20px;按照我们想的应该两个元素相距40px才对是吧，但实际上两个元素相距20px

```
.box1 {
    width: 100px;
    height: 100px;
    background-color: red;
    margin: 100px;
}

.box2 {
    width: 100px;
    height: 100px;
    background-color: blue;
    margin: 100px; 
}

<div class="box1"></div>
<div class="box2"></div>
```

这时就会产生外边距塌陷的问题：
这时外边距会怎么计算呢？
1.两个都是正数，取较大的值
2.两个都是负数，取绝对值较大的值
3.一正一负，取两个值得和
我们把修改下

```
.box1 {
    width: 100px;
    height: 100px;
    background-color: red;
    margin: 100px;
}

.box2 {
    width: 100px;
    height: 100px;
    background-color: blue;
    margin: 100px; 
}

.box{
    overflow:hidden;
}


<div class="box1"></div>
<div class="box">
    <div class="box2"></div>
</div>
```

 这样边距就是200px了

3、自适用两列布局

```
.box {
    width: 300px;
    height: 400px;
    background-color: skyblue;
}
.box1 {
    float: left;
    width: 100px;
    height: 100px;
    background-color: red;
} 
.box2 {
    height: 200px;
    background-color: blue;
}


<div class="box">
   <div class="box1"></div>
   <div class="box2"></div>
</div>
```

我们可以看到，蓝色部分被遮挡住了，也不是自动适应剩下的布局
如果我们想实现，怎么写呢
给box2加一个overflow: hidden;在来看下效果
自动适应布局
这个其实利用的就是 bfc特性bfc区域不会与float-box区域重叠
BFC当然还有一些别的用处 比如利用overflow:hidden去除浮动等
