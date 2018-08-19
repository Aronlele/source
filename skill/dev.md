title: "前端神器—Google Chrome Devtools细节详解"

date: 2018-08-19 21:18:23 +0800

update: 2018-08-19 21:18:23 +0800

tags:

- 技术笔记

------

### 这几个月发现了Google Chrome Devtools的易用性非常高的小功能和小细节 特意分享给大家、

## **Element**



### **1. styles内 悬浮样式名可以高亮符合样式名的所有元素（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3ojEYBnJcf38uwmnS0lSpniaYlzkGUG0JzQtalYYkIGaZjJLU8rGciaoKw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个功能还是比较有用的，适用于查看自己写的样式到底用在了哪些元素上，方便看到样式是否起冲突。



### **2. 快速调节样式的数值（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oiadyg13m1CKML4YYmnjh4SRjTHVGoqD6ofted6n8icbU45rDMNcRjc6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



tip：方向上下键就可以调整样式数值 再也不用靠猜了



### **3. 对于字体颜色，背景颜色等需要用到色值的，可以直接在控制台进行取色或编辑（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3owzM8tPwVH5o36Kyb5LC3IiaiaHguBDDrCWYhnlgjPnpYXoFxPyK9G2kQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个不仅可以手动取色，还可以再次取网页内其他元素的颜色

###  

### **4. 用工具模拟css系列操作（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oUjrdQgspGb5VQtNxdv9sovNicicibcm7C1xMLdFWNgfh8HTh54u7LXzvQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个比较有意思...有些鼠标悬浮的样式，鼠标移出就没有了。对于解决无法停留保存样式的问题，这样做还是很简单的，而且便于操控。



### **5. 图形化CSS动画编写（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oicdoBicibz86tW2DGZTRZuXnJMR2uL1Ml0GzPibuEaHiciasb5pGdn3e3qsA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个我具体没用过哈....因为很少写动画....但是看起来可以图形化调整运动曲线，比较有意思



### **6. CSS阴影图形化编写**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oeo3FhJJfLwL7Z5GhJibkmMj45ZHE2tv4PuTiaeaza94IJbehJQiauKQ9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个可以图形化调整阴影的XY偏移度，模糊程度，扩散程度



### **7. 快速查看选中元素的信息（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3o6b8Bq1VZMdxx90rGGv6WnCb0B5Ibk3IbI9YVGFjOichHZyvmb0siaAhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个有点意思，之前大家在做一些业务时，不少情况下需要console.log一下元素。看看他的属性。还可以看到选中元素的子元素，高度等等



## **Console**



### **1. 输出内容过滤器**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oWSWhQAjayvLO5BWiba86vE2PvkK4qmLw8Zsk68Ux4AWW5mtxwgaaXqA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个可以快速输入关键字 查找你想要的console是否支持正则自测

### 

### **2. Console优先级筛选**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oibD88kSObKnkxrpiaMB3ePUr7NkIEiceNO2ot8YT9A7WibdNve9MeFomiag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个可以快速筛选console的优先级，看到你想看到的调试信息



### **3. Console内的其他工具**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oaBkvqRZQXyxoRtKgdn0Qd7gtzTESmiaSBHICyDh3gM3wn2iaa4l2GkqA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这里面包含了“调试信息带时间戳”“显示XMLHttpRequests请求情况”等等，请自测



## **Network**

###  

### **技巧**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3ozzviaibJ2Y9ZicoR0xD53tLIGHX1at7jUiaFLnfS1zpgYP1RElCLU9zibicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这里面包含了“隐藏data开头的小图片链接”，“开启离线模式”，“对页面限速限制延迟”等一系列功能



## **Vue DevTools（其他框架开发者请略过）**



### **1. 快速定位组件的相对定位位置和dom树位置（如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oyymA19jmtwBvSwqZDScrBl9qEiceEFuAkibLpUicN7ibKI1JqpNbHX9TAA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



### **2. 快速查看元素的组件名 并定位组件树位置**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3ouFDgAtfR0aTHvJKsxLCZlKDTib2k3oOYpwiaLbfWLAt0tR3LBhcIlpsA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



### **Security**



可以看到页面加载以来所有请求的domain（不是hosts更正一下）



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oViciadOBTejWYpY0SfEOiaITRZRHKmKQicYmafg31Kcmic8Ut4EYYOPvklg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



## **更多工具**



请如图寻找 我要开始了咳咳



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3otwOzVGzbXsI4AS7fo7U9GKHoV0UrEkbGxGIGDjGR6SChSo6l6UTapQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



## **Animations**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oZJTwiaZQmD8q6DYQsL47xYLewsZHHgmqrsae4CibjW5eicicykjTAT0Ubw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



打开这个组件，触发页面动画后会出现如图上。

可以控制动画速度，拦截动画播放，手动拉动动画进行慢速播放。



## **Change**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oNGjx1IKhIIhAnIs7kdib8aSsCZT5Yb5SmJ2Prbnnbo0WqY4zicRIfm6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个非常有用啊啊啊啊啊啊啊。尤其是你在页面修改css后，回过头就忘记修改了哪一些......这个工具可以看到页面加载以来所有你的临时手动修改



## **Network conditions**



### **1. 基本操作（功能如图）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oVHEfEjRmkk3Z4ju1wwXkdQUHdSneDzDV01UrGS1Cm9Y2BXrTJIcBJg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)





### **2. 对页面进行限流（使用系统预设）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3omdAFlmCIRkLGxBxqQ3s3fBs2Zia8gdZsibPU9saficZmgAKpM336dNbsw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



### **3. 对页面进行限流（使用自定义）**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3ob6DKXWicxLMHfo6gib4dES9V6GheocSR4FhTZ1wBiaosibtoA11ZvnjouQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

点击上一张图的 "Add" 后，点击本章图的“Add custom profile”就会出现上图的情况 根据实际情况调整~~



## **Quick source**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3otex2iaERHKLmDDTWwFab0kxUgGY8ZWhq0Oc5YodBCXCK92xFiadGzkxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



在这里面写代码 边写边生效



## **Rendering**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oiagabXdpuybYcA8HsFOiclcZSvuNPqqzv4dKibiaQMahoZfAaDXATHCDsA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这里是各种监视器的地方，包括fps帧率监视器。

做HTML5/移动端游戏开发必备~~可以实时看到FPS，GPU的Memory占用~~



## **Search**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oecIXS8ibI5dSc1ruiaWpfX49GvOKZ2joeWQnzRYXJM22BghUYf28gLOw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



输入内容，全局搜索（所有加载的文件内容）



## **Sensors**



![img](https://mmbiz.qpic.cn/mmbiz_png/53hxia5OmNYL9k99CNVIcFhh3WXCJJQ3oChUTlI0sFicibka86ibUQbXmblmndfLjicenU29QArwJcEI0HoPMHxpz8g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)



这个可以模拟当前设备的位置，还可以模拟手机水平传感器参数。只需要拖动图中的手机就好