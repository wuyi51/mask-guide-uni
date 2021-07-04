# mask-guide-uni
基于uni-app的引导组件

目前仅支持微信小程序平台，其它平台未作测试和兼容

## 1.0.0 （2021-07-04）
----
使用方法
```
<mask-guide-uni ref="maskGuide">
 <view slot='nextButton'>next</view>
</mask-guide-uni>

this.$refs.maskGuide.start({
	textStyle:{
		fontSize: '24rpx',
	},
	showNextButton: false
})
```

字段|是否必须|说明
-----|------|------
callback|是|确认按钮的回调函数
mode|否|1:仅密码  2:仅指纹  3:指纹密码可切换 
passwordTitle|否|标题（输入密码）
fingerprintTitle|否|标题（指纹）
types|否|默认显示方式 1密码 2指纹
confirmText|否|确认按钮文本
cancelText|否|取消按钮文本
fingerprintDesc|否|指纹面板描述
