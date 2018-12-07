---
layout: post
title: 小程序scroll-view动态设置高度问题
date: 2018-12-07 16:25:15
categories: dev
tags: 微信小程序 
---


- 小程序很多控件都有内置的样式，可能会出现设置的样式表现与纯W3C标注表现不同；这里记录下很可能出现的问题，方便的处理方式；

- 这里要做的需求是，page中top有一个50px高度的header-bar，box占据其余部分高度（里面放一个可滚动的列表，也就是scroll-view）

由于不同设备高度不同，所以box部分的高度需要是动态的；
有两种处理方式:

###### 第一种header样式写死高度50px，在js的data中添加一个属性，在页面生命周期函数onload中计算
```
<view class='header'>
</view>
<view class='box' style='height:{{boxHeight}}px;'>
</view>
```

```
/**
   * 页面的初始数据
   */
  data: {
    boxHeight: 0
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

    let res = wx.getSystemInfoSync();
    let boxHeight = res.windowHeight - 50;
    this.setData({
      'boxHeight': boxHeight
    });

    wx.setNavigationBarTitle({
      title: '小程序中动态计算高度',
    })
  }
```
```
page {
  width: 100%;
  height: 100%;
  background-color: plum;
}

.header {
  background: red;
  height: 50px;
}

.box {
  background: yellowgreen;
}
```

###### 上一种方式需要计算比较麻烦，特别是有些根据网络请求回来的内容撑高，还可能要考虑一些数据不够、请求失败等问题，第二种是利用样式来自动拉伸，如使用flex布局
```
<view class='header'></view>
<view class='box' ></view>
```
```
page {
  width: 100%;
  height: 100%;
  background-color: plum;
  display: flex;
  flex-direction: column;
}

.header {
  background: red;
  height: 50px;
}

.box {
  background: yellowgreen;
  flex: 1;
}
```
![11.png](https://upload-images.jianshu.io/upload_images/1074666-8df66a94d61cac74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到预览完全没问题，接下来往里面放一个滚动视图scroll-view，设置为可以垂直滚动，高度设置为100%样式
```
<view class='header'>
</view>
<view class='box'>
  <scroll-view class='sv' scroll-y='true' style='height:100%;'>
  <view>韦神和这位水友打了照面，这样近的距离下连开十枪，但居然全被这位水友完美躲过。连韦酱都露出了一脸不可置信的表情！</view>
  <view>网友们则不仅因为这位水友的身法表示赞叹，纷纷表示，"被打倒的人总说别人的枪法有多好！拜托，只是你自己的身法没到家好吗！"大家也从韦神被秀一事中收获了无限乐趣。</view>
  <view>不过，也有网友努力为韦神堪称人体描边挂的十枪辩解，都是因为这位水友喜欢韦神太久了，也太了解韦神了，"当喜欢一个人久了...他的身法、他的枪法、他打的每一颗子弹，甚至每一颗子弹走的路径，我都会了如指掌，所以韦酱他...伤不了我！"</view>
  <view>对此，韦神也转发肯定了这个逻辑，并推荐各位网友称："男女朋友的可以试一试，打中一枪，说明他不够爱你！" 既然有韦神大力推荐，各位非单身的玩家们不妨一试啊，吃鸡究竟是不是一个爱情杀手游戏就能见分晓了！ 刚不过枪？落地成盒？手把手教你成功吃鸡！快关注兔玩英雄训练营返回搜狐，查看更多
  </view>
  <view>韦神和这位水友打了照面，这样近的距离下连开十枪，但居然全被这位水友完美躲过。连韦酱都露出了一脸不可置信的表情！</view>
  <view>网友们则不仅因为这位水友的身法表示赞叹，纷纷表示，"被打倒的人总说别人的枪法有多好！拜托，只是你自己的身法没到家好吗！"大家也从韦神被秀一事中收获了无限乐趣。</view>
  <view>不过，也有网友努力为韦神堪称人体描边挂的十枪辩解，都是因为这位水友喜欢韦神太久了，也太了解韦神了，"当喜欢一个人久了...他的身法、他的枪法、他打的每一颗子弹，甚至每一颗子弹走的路径，我都会了如指掌，所以韦酱他...伤不了我！"</view>
  <view>对此，韦神也转发肯定了这个逻辑，并推荐各位网友称："男女朋友的可以试一试，打中一枪，说明他不够爱你！" 既然有韦神大力推荐，各位非单身的玩家们不妨一试啊，吃鸡究竟是不是一个爱情杀手游戏就能见分晓了！ 刚不过枪？落地成盒？手把手教你成功吃鸡！快关注兔玩英雄训练营返回搜狐，查看更多
  </view>
</scroll-view>
</view>
```
```
page {
  width: 100%;
  height: 100%;
  background-color: plum;
  display: flex;
  flex-direction: column;
}

.header {
  background: red;
  height: 50px;
}

.box {
  background: yellowgreen;
  flex: 1;
}
```
![2.png](https://upload-images.jianshu.io/upload_images/1074666-1ec39578ed376e9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 问题：可以看到一旦加入scroll-view组件，样式被破坏了，header的高度莫名其妙变成了0；
- 疑问：小程序的说法是要求scroll-view一定要给一个固定的高度，不然就不行，难道只能用第一种方法来实现这个布局了？

我发现一个tip，其实只要给外围的box一个高度即可，随便一个高度，因为设置了flex拉伸级别，这个高度不影响拉伸；在H5中是没意义的，但是这里可以解决问题；
- ####最终给一个完整版本
```
<view class='header'>
  <view>财经</view>
  <view>股票</view>
  <view>商业</view>
  <view>汽车</view>
  <view>商业</view>
</view>
<view class='box'>
  <scroll-view class='sv' scroll-y='true'>
    <view>韦神和这位水友打了照面，这样近的距离下连开十枪，但居然全被这位水友完美躲过。连韦酱都露出了一脸不可置信的表情！</view>
    <view>网友们则不仅因为这位水友的身法表示赞叹，纷纷表示，"被打倒的人总说别人的枪法有多好！拜托，只是你自己的身法没到家好吗！"大家也从韦神被秀一事中收获了无限乐趣。</view>
    <view>不过，也有网友努力为韦神堪称人体描边挂的十枪辩解，都是因为这位水友喜欢韦神太久了，也太了解韦神了，"当喜欢一个人久了...他的身法、他的枪法、他打的每一颗子弹，甚至每一颗子弹走的路径，我都会了如指掌，所以韦酱他...伤不了我！"</view>
    <view>对此，韦神也转发肯定了这个逻辑，并推荐各位网友称："男女朋友的可以试一试，打中一枪，说明他不够爱你！" 既然有韦神大力推荐，各位非单身的玩家们不妨一试啊，吃鸡究竟是不是一个爱情杀手游戏就能见分晓了！ 刚不过枪？落地成盒？手把手教你成功吃鸡！快关注兔玩英雄训练营返回搜狐，查看更多

    </view>
    <view>韦神和这位水友打了照面，这样近的距离下连开十枪，但居然全被这位水友完美躲过。连韦酱都露出了一脸不可置信的表情！</view>
    <view>网友们则不仅因为这位水友的身法表示赞叹，纷纷表示，"被打倒的人总说别人的枪法有多好！拜托，只是你自己的身法没到家好吗！"大家也从韦神被秀一事中收获了无限乐趣。</view>
    <view>不过，也有网友努力为韦神堪称人体描边挂的十枪辩解，都是因为这位水友喜欢韦神太久了，也太了解韦神了，"当喜欢一个人久了...他的身法、他的枪法、他打的每一颗子弹，甚至每一颗子弹走的路径，我都会了如指掌，所以韦酱他...伤不了我！"</view>
    <view>对此，韦神也转发肯定了这个逻辑，并推荐各位网友称："男女朋友的可以试一试，打中一枪，说明他不够爱你！" 既然有韦神大力推荐，各位非单身的玩家们不妨一试啊，吃鸡究竟是不是一个爱情杀手游戏就能见分晓了！ 刚不过枪？落地成盒？手把手教你成功吃鸡！快关注兔玩英雄训练营返回搜狐，查看更多

    </view>
  </scroll-view>
</view>
```
```
page {
  width: 100%;
  height: 100%;
  background-color: plum;
  display: flex;
  flex-direction: column;
}

.header {
  background: red;
  height: 50px;
  width: 100%;
  display: flex;
}

.header > view {
  flex: 1;
  text-align: center;
  line-height: 50px;
  color: white;
}

.box {
  background: yellowgreen;
  flex: 1;
  height: 100px;
}

.sv {
  background-color: #e9e9;
  height: 100%;
}
```
![3.png](https://upload-images.jianshu.io/upload_images/1074666-91bb3930c73556b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)