# mask-guide-uni
基于uni-app的引导组件

目前仅支持微信小程序平台，其它平台未作测试和兼容

## 1.0.0 （2021-07-04）
----

目标容器

* 通过 `id="guide_顺序` 实现引导顺序
* `data-guide-text` 属性设置提示文字

```
<view id="guide_0" data-guide-text="提示1"></view>
<view id="guide_1" data-guide-text="提示2"></view>
```

组件使用方法

* 调用`start()`方法
* 通过`slot='nextButton'`可自定义 下一步 按钮

```
<mask-guide-uni ref="maskGuide">
  <view slot='nextButton'>我知道啦~</view>
</mask-guide-uni>

this.$refs.maskGuide.start({
  textStyle:{
    fontSize: '24rpx',
  },
  showNextButton: false
})
```

start参数说明

字段|是否必须|类型|默认值|说明
-----|------|------|------|------
textStyle|否|Object|{}|提示文字样式
showNextButton|否|Boolean|true|是否显示 '我知道啦' 按钮 
