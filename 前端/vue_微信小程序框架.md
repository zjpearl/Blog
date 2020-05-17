​	



### 微信小程序框架

###### 逻辑层

1.用来处理业务逻辑

2.JavaScript

###### 视图层

1.用来渲染页面

2.视图层描述语言wxml

3.视图层样式wxcss

###### 微信小程序框架=逻辑层+视图层+json配置文件

##### 目录结构

###### 页面目录结构

4个文件：

**app.js**定义全局数据和函数的使用：

1.指定微信小程序的周期函数

2.生命周期函数可以理解为微信小程序自己定义的函数：如：

onLaunch监听小程序初始化

onShow监听小程序显示

onHide监听小程序隐藏

**app.json**

1.配置页面路径

2.配置窗口表现

3.配置标签导航

4.配置网络超时

5.配置Debug模式

**app.wxss**定义全局样式文件，局部修饰文件优先

**project.config.json**

小程序项目各个性化配置文件；开发者针对各自喜好做一些个性化配置，如界面颜色，编译配置等

换机器重新安装开发工具需要重新配置

每一个项目根目录下均会生成此文件

###### 框架页面结构

放置在pages下

文件类型 | 是否必填 | 作用

#### 小程序注册

1.在页面里调用app.js全局数据

2.在页面js文件，按如下所示方法，就可以调用到全局数据globalData

  var APPInstance=getApp（）

 console.log(AppInstance.globalData)

3.不仅可以调用全局数据，还可以调用自定义的函数，但是不要调用生命周期函数

4.APP()必须在app.js中注册，且不能注册多个

5.不要在定义app()内的函数中调用getApp(),使用this就可以获取App实例

6.不要在onLoad的时候调用getCurrentPage(),page还没有生成

7.通过getApp()时，不要私自调用生命周期函数

###### 页面初始化数据

1.data为页面初始化数据

2.初始化数据将作为页面的第一次渲染

3.data会以json的形式由逻辑层传到渲染层

4.其数据必须是可以转成json的格式

​     字符串、数字、布尔值、对象或数值

5.渲染界面可通过WXML对数据进行绑定

###### 生命周期函数

onLoad页面加载：一个页面只调用一次，接收页面参数可以获取wx.navigateTo和wx.redirectTo及<navigateor>中query

onshow页面显示：每次打开页面都会调用一次

onReady页面初次渲染完成：一个页面只调用一次，可和图层进行交互，对界面的设置如wx.setNavgationBarTitle,请在onReady之后设置

onHide页面隐藏：当调用navigateTo或底部切换时调用

onUnload页面卸载，当调用redirectTo或navigateBack的时候调用

###### 组件属性绑定

将data里的数据绑定到微信小程序的组件上

``` //wxml
//wxml
<view id="item-{{id}}"></view>
//js
Page({
 data:{
 id:0
 }
})
```

###### 控制属性绑定

用来进行进行 if语句条件判断，如果条件满足，则执行

``` 
//wxml
<view wx:if="{{condition}}"></view>
//js
Page({
	data:{
	condition:true
	}
})
```

######  关键字绑定

常用于一些关键字

checked关键字

示例

```
<checkbox checked="{{false}}"></checkbox>

```

注意：不要直接写checked=“false”是个字符串

###### 运算

三元运算

``` 
<view hidden="{{flag?true:false}}">Hiden</view>
```

数学运算

```
<view>{{a+b}}+{{c}}+d</view>
Page({
	data:{
	a:1
	b:2
	c:3
	}
	
})
3+3+d
```

逻辑判断

```
<view wx:if={{length>5}}></view>
```

字符串运算

```
<view>{{"hello"+name}}</view>
```

数据路径运算

``` 
<view>{{object.key}}{{array[0]}}</view>
Page({
	data:{
	 object:{
	  key:'微信小程序'
	 }，
	 array:['hello']
	}
})
```



###### 条件渲染

1.使用 wx:if

2.使用wx:elif和wx:esle

```
<view wx:if={{length>5}}></view>
<view wx:elif={{length>2}}></view>
<view wx:else></view>
```

2.block wx:if判断多个组件

block不是一个组件，它仅是一个包装元素，不会再页面做任何渲染，只接受控制属性；不要在这个标签上绑定其他属性，如data-或者bindtap

``` 
<block wx:if="{{true}}">
 <view>view1</view>
 <view>view1</view>
</block>
```

###### 列表渲染

wx-for

数组中的数据值重复渲染 ，数组下标 index

```
<view wx:for="{{array}}">
	{{index}}:{{item.message}}
</view>
Page({
	data:{
	   array:[{
	   		message:'第0个元素'
	   },{
	   		message:'第1个元素'
	   }]
	}
})
```

使用wx:for-item可以指定数组当前元素的变量名

使用wx:for-index可以指定数组当前下标的变量名

```
  <view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
  	{{idx}}:{{itemName.message}}
  </view>
```

block wx:for列表渲染:渲染多个组件

wx:key指定唯一标识符

​	两种：1、字符串 2、保留关键字

```
 <switch wx:for="{{objectArray}}" wx:key="unique">{{item.id}}</switch>
Page:({
 objectArray:[
      {id:3,unique:'unique_3'},
      {id:2, unique: 'unique_2'},
      {id: 1, unique: 'unique_1'},
      { id: 0, unique: 'unique_0'},
    ]
  })
```

注意：如不提供wx:key ,会报一个warning，如果明确知道该列表是静态的，或者不必关心其顺序，可以选择忽略

###### 定义模板

WXML提供模板（template）

在<template/>定义代码片段，使用name属性，作为模板的名字

使用

在WXML 文件里，使用is属性，声明需要使用的模板，将模板所需的data传入

```
  <!-- template模板 -->
    <template name="msgItem">
      <view>
        <text>{{index}} : {{msg}}</text>
        <text> Time :{{time}}</text>
      </view>
    </template>
    <!-- 使用 -->
    <template is="msgItem" data="{{item}}"/>
```

is属性可以使用三元运算语法，来动态决定具体需要渲染哪个模板

```
   <template name="odd">
      <view>奇数</view>
    </template>
    <template name="even">
      <view>偶数</view>
    </template>
    <block wx:for="{{[1,2,3,4]}}">
    <template is="{{item%2 == 0 ? 'even':'odd'}}"/>
    </block>
```

#####  关于微信小程序的引用功能

import、include

import 模板文件

include模板之外的文件

##### WXS小程序脚本语言

if语句、switch条件语句、for循环语句（支持break、continue关键字）、while循环语句（当型、直到型）

#### 构建UI组件

##### 视图容器组件

##### view视图容器

1,WXML界面布局与HTML中的div

|       属性       | 类型    | 默认值 | 说明                                             |
| :--------------: | ------- | ------ | ------------------------------------------------ |
|      hover       | Boolean | false  | 是否启用单击                                     |
|   hover-class    | String  | none   | 指定按下去的样式表，当值为none时，没有单击态效果 |
| hover-start-time | Number  | 50     | 按住后多久出现单击态，单位为毫秒                 |
| hover-stay-time  | Number  | 400    | 手指松开后单击态保留时间，单位为毫秒             |
|                  |         |        |                                                  |



##### scroll-view可滚动视图区域

横向滚动或者纵向滚动

|       属性        | 类型        | 默认值 | 说明                                                         |
| :---------------: | ----------- | ------ | ------------------------------------------------------------ |
|     scroll-x      | Boolean     | false  | 允许横向滚动                                                 |
|     scroll-y      | Boolean     | false  | 允许纵向滚动                                                 |
|  upper-threshold  | Number      | 50     | 距顶部/左部多远时（单位为px），触发scrolltoupper事件         |
|  lower-threshold  | Number      | 50     | 距底部/右部多远时（单位为px），触发scrolltoupper事件         |
|    scroll-top     | Number      |        | 设置竖向滚动条设置                                           |
|    scroll-left    | Number      |        | 设置横向滚动条设置                                           |
| scroll-into-view  | String      |        | 值为某子元素id，则滚动到该元素，元素顶部对bind               |
| bindscrolltoupper | EventHandle |        | 滚动到顶部/左边，会触发scrolltoupper事件                     |
| bindscrolltolower | EventHandle |        | 滚动到底部/右边，会触发scrolltoupper事件                     |
|    bindscroll     | EventHandle |        | 滚动时触发，event.detail={scrollLeft,scrollTop/scrollHeight、scrollWidth、deltaX、deltaY} |

###### swiper滑块视图

用来在指定区域内切换显示内容、常用来制作海报轮播效果和页签内容切换效果

```
<view class="haibo">
  <swiper indicator-dots="{{indicatorDots}}" autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">
  <block wx:for="{{imgUrls}}">
   <swiper-item>
      <image src="{{item}}" class="silde-image" style="width:100%"></image>
   </swiper-item>
  </block>
</swiper>
</view>
 data: {
     indicatorDots:true,
    autoplay:true,
    interval:3000,
    duration:1000,
    imgUrls: ['../../images/s-1.png', '../../images/s-2.png', '../../images/s-3.png'],
    }
```

```
<!-- 页面切换 -->
<view class="content">
  <view class="loginTitle">
    <view class="{{currentTab==0?'select':'default'}}" data-current="0" bindtap="switchNav">手机登录</view>
    <view class="{{currentTab==1?'select':'default'}}" data-current="1" bindtap="switchNav">邮箱登录</view>
    <view class="{{currentTab==2?'select':'default'}}" data-current="2" bindtap="switchNav">验证码登录</view>
  </view>
  <view class="hr"></view>
  <swiper current="{{currentTab}}" style="height:{{winHeight}}px">
    <swiper-item>
      <view style="marginTop:10px;boder:1px solid grey;width:99%;height:200px">手机登录</view>
    </swiper-item>
    <swiper-item>
      <view style="marginTop:10px;boder:1px solid grey;width:99%;height:200px">邮箱登录</view>
    </swiper-item>
    <swiper-item>
      <view style="marginTop:10px;boder:1px solid grey;width:99%;height:200px">验证码登录</view>
    </swiper-item>
  </swiper>
</view>

.loginTitle{
  display: flex;
  flex-direction: row;
  width: 100%;
}
.select{
  font-size: 12px;
  color: black;
  width: 33%;
  text-align: center;
  height: 45px;
  line-height: 45px;
  border-bottom: 5rpx solid green;
}
.default{
  font-size: 12px;
  margin: 0 auto;
  padding: 15px;
}
.hr{
  border:1px solid #ccc;
  opacity: 0.2;
}

  //页面切换
    currentTab:0,
    winHeight:0,
    winWidth:0,
    
    
      onLoad: function (options) {
    // 页面切换
    var page = this;
    wx.getSystemInfo({
      success: function(res) {
        console.log(res);
        page.setData({
          winWidth:res.windowWidth
        });
        page.setData({
          winHeight:res.windowHeight
        });
      },
    })
  },
    /**
   * 页面切换
   */
  switchNav:function(e){
      var page = this;
      if(this.data.currentTab == e.target.dataset.current){
        return false;
      }else{
        page.setData({
          currentTab:e.target.dataset.current
        })
      }
  },
```

![1581473839112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581473839112.png)



![1581473859260](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581473859260.png)

###### movable-view可移动视图

1、可移动容器、可以在页面做拖曳滑动

2、使用该组件是，需要先定义可涌动区域movable -area，然后定义直接子节点movable-view，否则不能移动

3、movable-area必须设置width、heigth属性，不设置默认为10px

4、movable-view必须设置width、heigth属性，不设置默认为10px、默认为绝对定位，top和left属性为0px;

**htouchmove指初次手指触摸后的移动横向移动**

**vtouchmove指初次手指触摸后的移动纵向移动**

注意：如果catch到这两个事件，则意味着touchmove事件也被catch

| 属性          | 类型        | 默认值 | 说明                                                         |
| ------------- | ----------- | ------ | ------------------------------------------------------------ |
| direction     | string      | none   | movable-view的移动方向，属性值有all，vertical,horizontal、none |
| inertia       | boolean     | false  | moval-view是否带有惯性                                       |
| out-of-bounds | boolean     | false  | 超过可移动区域后，movable-view是否还可以移动                 |
| x             | number      |        | 定义在x轴方向的偏移、如果x的值不在可移动范围内，会自动移动到可移动范围；改变x的值会触发动画 |
| Y             | number      |        | 定义在Y轴方向的偏移、如果Y的值不在可移动范围内，会自动移动到可移动范围；改变Y的值会触发动画 |
| damping       | number      | 20     | 阻尼系数，用于控制x或y改变时的动画和过界回弹的动画，值越大移动越快 |
| friction      | number      | 2      | 摩擦系数，用于控制惯性滑动的动画，值越大摩擦力越大，滑动越快停止，必须大于0 ，否则会被设置成默认值 |
| disabled      | boolean     | false  | 是否禁用                                                     |
| scale         | boolean     | false  | 是否支持双指标缩放、默认缩放手势生效区域是在movable-view内   |
| scale-min     | nunber      | 0.5    | 定义缩放信息最小值                                           |
| scale-min     | nunber      | 0.5    | 定义缩放信息最大值                                           |
| scale-value   | number      | 1      | 定义缩放信息倍数，取值范围为0.5-10                           |
| bindchange    | eventhandle |        | 拖动过程中触发的事件，event.detail={x,y,source}              |
| bindscale     | eventhandle |        | 缩放过程中触发的事件，event.detail={x,y,scale}               |



###### cover-view覆盖原生组件的视图容器

文本视图

**cover-view**是指覆盖在原生组件智商的文本视图，可覆盖的原生组件包括map、video、canvas、camera、只支持嵌套cover-view、cover-image

| 属性       | 类型          | 默认值 | 必填 | 说明                                                         |
| ---------- | ------------- | ------ | ---- | ------------------------------------------------------------ |
| scroll-top | number/string |        | 否   | 设置顶部滚动偏移量，仅在设置了overflow-y:scroll成为为滚动元素后生效 |



**cover-image**是指覆盖在原生组件之上的图片视图，可覆盖的原生组件通cover-view一样，支持嵌套在cover-view里

| 属性      | 类型        | 默认值 | 说明                                                         |
| --------- | ----------- | ------ | ------------------------------------------------------------ |
| src       | string      |        | 图标路径，支持临时路径、网络地址（1.6.0起支持）、云文件ID（2.2.3起支持）暂不支持base-64格式 |
| bindload  | eventhandle |        | 图片加载成功时触发                                           |
| binderror | eventhandle |        | 图片加载失败是触发                                           |



```
.controls{
  position: relative;
  top:50%;
  height: 50px;
  margin-top: -25px;
  display: flex;
}
.play,.pause,.time{
    flex:1;
    height:100%;
}
.time{
  text-align: center;
  background-color: rgb(0,0,0,0.5);
  color:white;
  line-height: 50px;
}
.img{
  width: 40px;
  height: 40px;
  margin: 5px auto;
}

<video class="myVideo" src="E:\homework\前端框架/00-01-准备.mp4" controls="{{false}}" event-model="bubble">
  <cover-view class="controls">
    <cover-view class="play" bindtap="play">
      <cover-image class="img" src="../../images/self.png"></cover-image>
    </cover-view>
    <cover-view class="pause" bindtap="pause">
      <cover-image class="img" src="../../images/self-l.png"></cover-image>
    </cover-view>
    <cover-view class="time">00:00</cover-view>
  </cover-view>
</video>

 onReady: function () {
    this.videoCtx = wx.createVideoContext('myVideo')

  },
  // 播放
  play: function () {
    this.videoCtx.play();
  },
  pause: function () {
    this.videoCtx.pause();
  },
  
  
```

![1581473887610](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581473887610.png)

#### 基础内容组件

icon图标

* 成功

* 警告

* 取消

* 提示

* 下载

  | 属性  | 类型          | 默认值 | 必填 | 说明                                                         |
  | ----- | ------------- | ------ | ---- | ------------------------------------------------------------ |
  | type  | string        |        |      | icon的类型，有效值：success、success_no_circle,info,warn,watings,cancle,download,search,clear |
  | size  | number/string |        |      | icon的大小、单位px                                           |
  | color | string        |        |      | icon的颜色，同css的color                                     |
  |       |               |        |      |                                                              |

  

  ```
  <!-- 图标组件 -->
  <view class="group">
    <block wx:for="{{iconSize}}">
      <icon type="success" size="{{item}}"></icon>
    </block>
  </view>
  <view class="group">
    <block wx:for="{{iconType}}">
      <icon type="{{item}}" size="40"></icon>
    </block>
  </view>
  <view class="group">
    <block wx:for="{{iconColor}}">
      <icon type="success" size="40" color="{{item}}"></icon>
    </block>
  </view>
  
   data: {
      // icon图标组件
      iconSize:[20,30,40,50],
      iconColor:["red","orange","bule"],
      iconType:["success","cancle","warn","waiting","download","search","clear"]
      
    },
  ```

  ![1581474146215](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581474146215.png)

text文本组件

1、文本组件支持转义字符“\”，如换行\n,空格\t

2、<text/>组件内只支持<text/>嵌套，除了文本节点以外的其他节点都无法长按选中

```
<!-- 文本组件 -->
<view>
  <text>微信小程序\t哈哈哈</text>
</view>
```



进度条组件

```
<!-- 进度条组件 -->
<progress percent="20" show-info></progress><text>\n</text>
<progress percent="40" stroke-width="12"></progress><text>\n</text>
<progress percent="60" color="pink"></progress><text>\n</text>
<progress percent="80" active></progress><text>\n</text>
```

rich-text富文本

使用HTML中元素、node节点等

```
<rich-text nodes="{{nodes}}" bindtap="tap"></rich-text>
 // rich-text富文本
    nodes:[{
      name:'div',
      attrs:{
        class:'div_class',
        style:'line-height:60px;color:red;'
      },
      children:[{
        type:'text',
        text:'你好！微信小程序'
      }]
    }]
    
    tap(){
  console.log('tap')
},
```



![1581475905557](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581475905557.png)

#### 表单组件

button按钮组件

| 属性             | 类型    | 默认值       | 说明                                                         |
| ---------------- | ------- | ------------ | ------------------------------------------------------------ |
| size             | string  | default      | 按钮的大小                                                   |
| type             | string  | default      | 按钮的样式类型                                               |
| plain            | boolean | false        | 按钮是否镂空，背景色透明                                     |
| disabled         | boolean | false        | 是否禁用                                                     |
| loading          | boolean | false        | 名称前是否loading图标                                        |
| form-type        | string  |              | 用于form组件，点击分别会触发submit、reset事件                |
| hover-class      | string  | button-hover | 指定按钮按下去的样式表，当后hover-class=“none”时，没有点击态效果 |
| hover-start-time | number  | 20           | 按下后多久出现点击态，单位为毫秒                             |
| hover-stay-time  | number  | 70           | 是指松开后点击态保留时间，单位为毫秒                         |



```
<!-- 按钮 -->
<button type="default" size="{{defaultSize}}" loading="{{loading}}" plain="{{plain}}" disabled="{{disabled}}" bindtap="default" hover-class="other-button-hover">default</button>
<button type="primary" size="{{primarySize}}" loading="{{loading}}" plain="{{plain}}" disabled="{{disabled}}" bindtap="primary" hover-class="other-button-hover">primary</button>
<button type="warn" size="{{warnSize}}" loading="{{loading}}" plain="{{plain}}" disabled="{{disabled}}" bindtap="warn" hover-class="other-button-hover">warn</button>
<button bindtap="setDisabled">点击设置以上按钮disabled的属性</button>
<button bindtap="setPlain">点击设置以上按钮plain的属性</button>
<button bindtap="setLoading">点击设置以上按钮loading的属性</button>
<button open-type="contact">进入客服会话</button>
<button open-type="getUserInfo" lang="hhhh" bindgetuserinfo="onGotUserInfo">获取用户信息</button>
<button open-type="openSetting">打开授权设置页</button>
```

![1581490581491](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581490581491.png)

checkbox-group多项选择器

1、有个绑定事件bindchange

2、<checkbox-group/>中选中项发生改变是触发change事件

3、detail={value：[选中的checkbox的value的数组]}

| 属性     | 类型 | 默认值 | 说明 |
| -------- | ---- | ------ | ---- |
| value    |      |        |      |
| disabled |      |        |      |
| checked  |      |        |      |
| color    |      |        |      |

```
<checkbox bindtap="checkboxChange">
  <label class="checkbox" wx:for="{{items}}">
    <checkbox value="{{item.name}}" checked="{{item.checked}}">{{item.value}}</checkbox>
  </label>
</checkbox>

  // chekbox
    items:[
      {name:'USA',value:'美国'},
      { name: 'USA', value: '中国',checked:'true' },
      { name: 'USA', value: '美国' },
      { name: 'USA', value: '美国' },
    ]
  checkboxChange:function(e){
  console.log('checkbox发生change事件，携带value值为：',e.detail.value)
},
```

![1581490597109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581490597109.png)

单选框radio

```
<!-- 单选框 -->
<radio-group class="radio-droup" bindchange="radioChange">
  <label class="radio" wx:for="{{items}}">
    <radio value="{{item.name}}" checked="{{item.checked}}">{{item.value}}</radio>
  </label>
</radio-group>
```

![1581490816078](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581490816078.png)

input 单行输入框

* text

* number

* IDcard

* digit

  三个常用事件

  * 输入时（bindinput）
  * 光标聚焦时（bindfocus）
  * 光标离开时（bindblur）



picker选择器



+ 时间

+ 地域

  **Slider滑动选择器**

+ bindchange -eventhandle

  **switch开关选择器属性**

  **form表单**

| 属性          | 类型        | 默认值 | 说明                                                         |
| ------------- | ----------- | ------ | ------------------------------------------------------------ |
| report-submit | boolean     | false  | 是否返回formId用于发送模板消息                               |
| bindsubmit    | eventhandle |        | 携带form中的数据触发submit事件，event.detail{value：{'name':'value'}} |
| bindreset     | eventhandle |        | 重置                                                         |

### 导航组件

* navigator页面链接组件

  1. 保留当前页跳转，跳转后可返回当前页，与wx.navigateTo效果一样

  2. 关闭当前页跳转，无法返回当前页，与wx.redirectTo效果一样

  3. 调换到底部标签导航指定的页面，与wx.switchtab跳转效果一样

     通过**open-type**

     

* wx.navigateTo保留当前页跳转

* wx.redirectTo关闭当前页跳转

* wx.switchTab跳转到tabBar页面

* wx.navigateBack返回上页

  通过getCurrentPages返回上几级页面

* 设置导航条

  wx.setNavigateBarTiltle动态设置当前页面的标题

  ### 媒体组件

  ###### audio音频

  返回错误码

  | 返回错误码 | 描述               |
  | ---------- | ------------------ |
  | 1          | 获取资源被用户禁止 |
  | 2          | 网络错误           |
  | 3          | 解码错误           |
  | 4          | 不合适资源         |

  ###### image图片

  * 缩放模式
  * 裁剪模式

  ###### video视频

  wx.createVideoContext

  **发送弹幕**
  
  ```
  <view class="section tc">
    <video src="{{src}}" controls></video>
    <view class="btn-area">
      <button bindtap="bindButtonTap">获取视频</button>
    </view>
  </view>
  <view class="section tc">
    <video id="myVideo" src="{{src}}" danmu-list="{{danmuList}}" enable-danmu danmu-btn controls></video>
    <view class="btn-area">
      <button bindtap="bindButtonTap">获取视频</button>
      <input bindblur="bindInputBlur"></input>
      <button bindtap="bindSendDanmu">发送弹幕</button>
    </view>
  </view>
  
  
  
  
  const app = getApp()
  function getRandomColor(){
    let rgb = []
    for(let i = 0;i<3;++i){
      let color = Math.floor(Math.random()*256).toString(16)
      color = color.length == 1 ? '0'+color:color
      rgb.push(color)
    
    }
    return '#' +rgb.join('')
  }
   // video
        src:'',
        danmuList:[
          {
            text:'第1s出现的弹幕',
            color:'#ff0000',
            time:1
          },
          {
            text:'第3s出现的弹幕',
            color:'#ff00ff',
            time:3
          }
        ],
  
  bindInputBlur:function(e){
    this.inputValue = e.detail.value
      console.log("ahahah")
  },
    bindButtonTap:function(){
    var that = this
    wx.chooseVideo({
      sourceType:['album','camera'],
      maxDuration:60,
      camera:['front','back'],
      success:function(res){
        that.setData({
          src: res.tempFilePath
        })
      },
    })
     
  },
    bindSendDanmu:function(){
    this.videoContext.sendDanmu({
      text:this.inputValue,
      color:getRandomColor()
    })
     
  },
  ```
  
  ![1581561589381](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581561589381.png)
  
  ###### camera相机
  
  live-player
  
  live-pusher实时音视频录制
  
  ###### 地图组件
  
  map地图组件用来开发与地图有关的应用
  
  在地图上可以标记覆盖以及制定一系列的坐标位置
  
  subkey
  
  ```
  <view class="page-section">
    <map id="map" longitude="106.33" latitude="29.35" scale="15" controls="{{controls}}" bindcontroltap="controltap" markers="{{markers}}" bindmarkertap="markertap" polyline="{{polyline}}" bindregionchange="regionchange" show-location style="width:100%;height:300px;"></map>
  </view>
  ```
  
  
  
  ![1581561725172](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581561725172.png)
  
  

#### 画布组件

canvas绘制组件用来绘制正方形，圆形等等

| 属性名          | 类型        | 默认值 | 说明                                                         |
| --------------- | ----------- | ------ | ------------------------------------------------------------ |
| canvas-id       | String      |        | canvas 组件的唯一标识符                                      |
| disable-scroll  | Boolean     | false  | 当在 canvas 中移动时且有绑定手势事件时，禁止屏幕滚动以及下拉刷新 |
| bindtouchstart  | EventHandle |        | 手指触摸动作开始                                             |
| bindtouchmove   | EventHandle |        | 手指触摸后移动                                               |
| bindtouchend    | EventHandle |        | 手指触摸动作结束                                             |
| bindtouchcancel | EventHandle |        | 手指触摸动作被打断，如来电提醒，弹窗                         |
| bindlongtap     | EventHandle |        | 手指长按 500ms 之后触发，触发了长按事件后进行移动不会触发屏幕的滚动 |
| binderror       | EventHandle |        | 当发生错误时触发 error 事件，detail = {errMsg: 'something wrong'} |

```
<!-- 画布组件 -->
<view class="container">
  <canvas style="width:300px;height:200px;" canvas-id="firstCanvas"></canvas>
  <!-- 当使用绝对定位时，文档流后边的canvas的显示层级高于前边的canvas -->
  <canvas style="width:400px;height:500px;" canvas-id="secondCanvas"></canvas>
  <!-- canvas-id与前一个canvas重复，该canvas不会显示，并会显示错误时间到APPservice -->
  <canvas style="width:400px;height:500px;" canvas-id="canvasIdErrorCallBack"></canvas>
</view>

 /**
     * 画布
     */
  canvasIdErrorCallBack: function () {
    console.error(e.detail.errMsg)
  },
  
   onReady: function (e) {
    // 使用wx.createContext获取绘图上下文context
    3
    var context = wx.createCanvasContext('firstCanvas')
    // 矩形
    context.setStrokeStyle("#00ff00")
    context.setLineWidth(5)
    context.rect(0, 0, 200, 200)
    context.stroke()


    context.setStrokeStyle("#ff0000")
    context.setLineWidth(2)
    context.moveTo(160, 100)
    context.arc(100, 100, 60, 0, 2 * Math.PI, true)

    context.moveTo(140, 100)
    context.arc(100, 100, 40, 0,  Math.PI, false)

    context.moveTo(85, 80)
    context.arc(80, 80, 5, 0, 2 * Math.PI, true)

    context.moveTo(125, 80)
    context.arc(120, 80, 5, 0, 2 * Math.PI, true)
    
    context.stroke()
    context.draw()
  },

```

![1581564069773](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581564069773.png)





######  动画

 duration: 动画持续多少毫秒
      timingFunction: “运动”的方式，例子中的 'ease'代表动画以低速开始，然后加快，在结束前变慢 
      delay: 多久后动画开始运行

​      opacity() 慢慢变透明

​	 step()

 translate(100, -100) 向X轴移动100的同时向Y轴移动-100



![1581848547105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1581848547105.png)

translate(x,y) 定义 2D 转换。
translate3d(x,y,z)  定义 3D 转换。
translateX(x)   定义转换，只是用 X 轴的值。
translateY(y)   定义转换，只是用 Y 轴的值。
translateZ(z)   定义 3D 转换，只是用 Z 轴的值。
scale(x,y)  定义 2D 缩放转换。
scale3d(x,y,z)  定义 3D 缩放转换。
scaleX(x)   通过设置 X 轴的值来定义缩放转换。
scaleY(y)   通过设置 Y 轴的值来定义缩放转换。
scaleZ(z)   通过设置 Z 轴的值来定义 3D 缩放转换。
rotate(angle)   定义 2D 旋转，在参数中规定角度。
rotate3d(x,y,z,angle)   定义 3D 旋转。
rotateX(angle)  定义沿着 X 轴的 3D 旋转。
rotateY(angle)  定义沿着 Y 轴的 3D 旋转。
rotateZ(angle)  定义沿着 Z 轴的 3D 旋转。
skew(x-angle,y-angle)   定义沿着 X 和 Y 轴的 2D 倾斜转换。
skewX(angle)    定义沿着 X 轴的 2D 倾斜转换。
skewY(angle)    定义沿着 Y 轴的 2D 倾斜转换。



#### 小程序必备API

1. 小程序/服务器架构

   * 小程序和服务器通信的架构也可以称为C/S架构

   * 请求过程

     + 小程序先向服务器发送网络请求

     + 服务器收到请求后执行相关代码处理请求

     + 处理完毕后服务器向小程序回复并返回数据

     + 小程序相关接口将毁掉success（）函数并对拿到的数据进行处理

       注意事项：

       + 默认超时时间和最大超时时间都是60秒
       + 所涉及的三种接口API：request、upLoadFile、downLoadFile最大并发限制是10个
       + 小程序进入后台运行后，如果在5s内网络请求没有结束，会回调错误信息fail interrupted;在回到后台之前，网络接口调用都无法调用。

2. 服务器添加域名配置

   + 每一个小程序在指定域名进行网络通信前都必须将该域名添加到管理员后台白名单中

   + 配置方法：

     + 登录mp.weixin.qq.com

     + 点击左侧“开发”，“开发设置”下的“服务器域名”中配置域名

       注意事项：

       + 开发者可将填入自己或第三方的域名地址

       + 域名只支持HTTPS（request、upLoadFile、downLoadFile）和wss(connectSocket)协议

       + 域名不能使用IP地址或localhost

       + 域名必须经过ICP备案

       + 每类接口分别可以配置最多20个域名

       + 域名每个月只可以申请修改5次

         跳过域名校验

         + 如果开发者暂时无法登记有效域名，可以在开发和测试环节暂时跳过域名校验

         + 方法

           + 点击开发者工具中的“详情”按钮

           + 点击“本地设置”设置选项卡

           + 勾选“不校验合法域名”项

             

3. 发起请求

   + 小程序使用wx.request(OBJECT)发起网络请求

   + wx.request参数说明

     | 属性         | 类型                      | 默认值 | 必填 | 说明                                                         |
     | ------------ | ------------------------- | ------ | ---- | ------------------------------------------------------------ |
     | URL          | string                    |        | 是   | 开发者服务器接口地址                                         |
     | data         | string/object/ArrayBuffer |        | 否   | 请求的参数                                                   |
     | header       | Object                    |        | 否   | 设置请求的header、header中不能设置Referer.content-type默认为application/json |
     | method       | string                    | get    | 否   | http请求方法                                                 |
     | dataType     | string                    | json   | 否   | 返回的数据格式                                               |
     | responseType | string                    | text   | 否   | 返回的数据类型                                               |
     | success      | function                  |        | 否   | 接口调用成功的回调函数                                       |
     | fail         | function                  |        | 否   | 接口调用失败的回调函数                                       |
     | complete     | function                  |        | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

     

**文件的上传下载**

+ 文件的上传功能需要配合开发者使用
+ 文件下载功能使用开发者服务器或第三方均可
+ wx.uploadFile文件上传（向服务器端发送HTTPS Post请求
+ wx.downloadFile文件下载

文件上传

```
<view class="page-body">
  <view class="demo-box">
    <view class="title">文件上传演示</view>
    <image wx:if='{{src}}' src='{{src}}' mode="widthFix"></image>
    <button bindtap="chooseImage">选择文件</button>
    <button type="primary" bindtap="uploadFile">开始上传</button>
  </view>
</view>
data: {
    src:"",//上传图片的路径地址
  },
  chooseImage:function(){
    var that  = this
    wx.chooseImage({
      count :1,//默认9
      sizeType:['original','compressed'],//可以指定原图还是亚速途，默认二者都有
      sourceType:['album','camera'],//可以指定来源是相册还是相机，默认二者都有
      success: function(res) {
        let src = res.tempFilePaths[0]
        that.setData({
          src:src
        })
      },
    })
  },
  /**
   * 上传文件
   */
  uploadFile:function(){
    var that = this
    let src = this.data.src
    if(src ==""){
      wx.showToast({
        title:'请先选择文件！',
        icon:'none'
      })
    }else{
      //发起文件上传请求
      var uploadTask = wx.uploadFile({
        url: '',//开发者自己的服务器地址
        filePath: 'src',
        name: 'file',
        success: function(res) {},
        fail: function(res) {},
        complete: function(res) {
          console.log(res)
          wx.showToast({
            title: res.data,
          })
        },
      })
      uploadTask.onProgressUpdate((res) =>{
        console.log('上传精度',res.progress)
        console.log('已经上传的数据长度',res.totalBytesSent)
        console.log('预期需要上传的数据总长度',res.totalBytesExpectedToSend)
      })
    }
  },
```

下载

```
<view class="page-body">
  <view class="demo-box">
    <view class="title">文件下载演示</view>
    <block wx:if='{{isDownload}}'>
      <image wx:if='{{src}}' src='{{src}}' mode="widthFix"></image>
      <button bindtap="reset">重置</button>
    </block>
    <button type="primary" bindtap="download">开始下载</button>
  </view>
</view>
   isDownload:false,

/**
   * 下载文件
   */
  download:function(){
    var that = this//开始下载
    var downloadTask = wx.downloadFile({
      url:'',//域名地址下的
      success:function(res){
        //只要服务器有响应数据，就会把响应内容写入success回调，业务需要自行判断是否下载到了想要的内容
        if(res.statusCode === 200){
          let src = res.tempFilePath//文件临时路径地址
          that.setData({
            src:src,
            isDownload:true
          })
        }
      }
    })
    //任务对象监听下载进度
    downloadTask.onProgressUpdate((res)=>{
      console.log('下载进度',res.progress)
      console.log('已经下载的数据长度',res.totalBytesWritten)
      console.log('预期需要下载的数据总长度',res.totalBytesExpectedToWrite)
    })
  },
  /**
   * 清空下载图片
   */
  reset:function(){
    this.setData({
      src:'',
      isDownload:false
    })
  },
```

#### WebSocket会话API

+ WebSocket会话用来创建一个会话连接，创建完会话连接后可以相互通信，像微信聊天和QQ群一样

  共涉及7个API的使用

  + wx.connectSocket(OBEJCT),创建一个会话连接

    ```
    wx.connectSocket({
    	url:'wss://example.qq.com'
    	header:{
    		'connnect-type':'application/json'
    	},
    	protocols:['protocol'],
    	      method:“GET”
    })
    ```

    

  + wx.onSocketOpen(CallBack),监听WebSocket连接打开时间

  + wx.onSokcetError(CallBack)，监听WebSocket错误

  + wx.sendSocketMessage(OBJECT)发送数据

  + wx.onSocketMessage（CallBack）监听WebSocket收到服务器的消息时间

  + wx.closeSocket(),关闭WebSocket连接

  + wx.onSocketClose（CallBack），监听WebSocket关闭

```
wx.connectSocket({
	url:'text.php'
})
//如果wx.connectSocket还没有回调wx.onSocketOpen,而先调用wx.closeSocket,那么久做不到关闭WebSocket的目的
//必须在WebSocket打开期间调用wx.closeSocket才能关闭
wx.onSocketOpen({
	wx.colseSocket()
})
wx.onSocketClose(function(res){
	console.log('WebSocket 已关闭！')
})
```

###### 图片处理API

+ wx.chooseImages

+ wx.previwImage

  ```
  onload:function(){
  	wx.previewImage({
  		current:'bye.jpg'//当前显示图片的http连接
  		urls:['bye.jpg','location.png']//需要显示图片http链接列表
  	})
  }
  ```

  

+ wx.getImageInfo获取图片信息

  + 包括图片信息
  + 图片的高度、宽度以及返回路径

+ wx.savaImageToPhotoAlbum

  ```
  onload:function(){
  	wx.savaImageToPhotoAlbum({
  		success(res){
  		
  		}
  	})
  }
  ```

  

###### 文件操作API

+ wx.saveFile保存文件到本地

+ wx.getSavedFileList获取本地文件列表

  ```
  onload:function(){
  	wx.getSaveFileList({
  		success(res){
  			var fileList = res.fileList;
  			console.log(fileList)
  			var file=fileList(o)
  			wx.getSavedFileListInfo({
  				filePath:file.filePath
  				success(res){
  					console.log(res.fileList)
  				}
  			})
  		}
  	})
  }
  ```

  

+ wx.getSavedFileInfo获取本地文件信息

+ wx.removeSavedFile删除本地文件

+ wx.openDocument打开文档

+ wx.getFileInfo获取文件信息

##### 数据缓存API

+ wx.getStorage(OBJECT)异步方式(会覆盖原来该key对应的内容)

+ wx.setStorageSync(KEY,DATA)同步方式

+ 数据存储上限10MB

  ```
  Page({
  	onload:function(){
  		var user=this.getUserInfo();
  		concole.log(user);
  		wx.setStorage({
  			key:'user',
  			data:user,
  			success:function(res){
  				console.log(res);
  			}
  		})
  	},
  	getUserInfo:function(){
  		var user=new Object();
  		user.name='zll'
  		user.sex='女'
  		user.age=20;
  		user.address='重庆市'
  		return user;
  	}
  })
  ```

  

```
Page({
	onload:function(){
		var userSync = this.getUserInfo();
		wx.setStorageSync('userSync',userSync)
		},
		getUserInfo:function(){
		var user = new Object();
		user.name='zll'
		user.sex='女'
		user.age=20
		user.address='重庆市'
		return user
		}
})
```

+ 获取本地缓存数据

  + wx.getStorage(OBJECT)

    ```
    onLoad:function(){
    	wx.getStorage({
    		key:'user'
    		success:function(res){
    		console.log(res)
    		}
    	})
    }
    ```

    

  + wx.getStorageSync(OBJECT)

    ```
    onLoad:function(){
    	var userSync = wx.getStorageSync('userSync')
        console.log(userSync)
    }
    ```

    

  + wx.getStorageInfo(OBJECT)

    ```
    onLoad:function(){
    	wx.getStorageInfo({
    	success:function({
    		console.log(res)
    	})
    	})
        }
    ```

    

  + wx.getStorageInfoSync(OBJECT)

```
onload:function(){
	var storage=wx.getStorageInfoSync();
	console.log(storage)
}
```

+ 移除和清理本地缓存
  + wx.removeStorage(OBJECT)从本地缓存中移除指定key
  + wx.removeStorageSync(KEY)从本地缓存中移除指定key
  + wx.clearStorage()清理本地数据缓存
  + wx.clearStorageSync()清理本地数据缓存

##### 地图API

​     

##### 登录API

