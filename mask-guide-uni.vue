<template>
	<view>
		<view v-if="showGuide" class="outer" @click="nextGuide" @touchmove.stop.prevent="moveHandle">
			<view class="inner" :style="[innerStyle]"></view>
			<view class="innerText" :style="[innerTextStyle]">
				<view v-if="nextBtnPosition === 'top' && showNextButton" class="next-btn-wrap">
					<slot name='nextButton'>
						<view class="next-btn">我知道啦~</view>
					</slot>
				</view>
				<text :style="[textStyle]">{{innerText}}</text>
				<view v-if="nextBtnPosition === 'bottom' && showNextButton" class="next-btn-wrap">
					<slot name='nextButton'>
						<view class="next-btn">我知道啦~</view>
					</slot>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "mask-guide-uni",
		data() {
			return {
				innerStyle: {},
				innerTextStyle: {},
				guide: 0,
				showGuide: false,
				innerText: '',
				deviceInfo: {},
				textStyle: {},
				showNextButton: true,
				nextBtnPosition: 'top'
			};
		},
		methods: {
			start(props) {
				for (let k in props) {
					this[k] = props[k];
				};
				this.deviceInfo = uni.getSystemInfoSync();
				this.showGuide = true;
				this.nextGuide();
			},
			moveHandle(){
				
			},
			nextGuide() {
				const index = this.guide;
				uni.createSelectorQuery().select(`#guide_${index}`).fields({
					id: true,
					dataset: true,
					rect: true,
					size: true,
					scrollOffset: true,
					context: true,
					properties: ['scrollX', 'scrollY'],
					computedStyle: ['borderRadius'],
				}, data => {
					if (data) {
						let {
							left,
							right,
							top,
							width,
							height,
							borderRadius,
							dataset
						} = data;
						//判断是否需要滚动
						const { windowHeight } = this.deviceInfo;
						this.innerStyle = {
							'left': `${left}px`,
							'top': `${top}px`,
							'width': `${width}px`,
							'height': `${height}px`,
							'borderRadius': borderRadius
						};
						//根据位置设置显示的位置
						const leftRpx = this.pxToRpx(left);
						let innerTextStyle = {
							'top': `${top + height + 5}px`,
						}
						if(leftRpx > 400){
							innerTextStyle.textAlign = 'right';
							innerTextStyle.right = `${750 - this.pxToRpx(right)}rpx`;
							innerTextStyle.marginLeft = `10px`;
						}else{
							innerTextStyle.textAlign = 'left';
							innerTextStyle.left = `${left}px`;
							innerTextStyle.marginRight = `10px`;
						}
						//如果长度够长 居中显示
						if(width > 300){
							innerTextStyle.width = `${width}px`;
							innerTextStyle.textAlign = 'center';
						}
						//内容在可视区域以下
						if(windowHeight < top){
							uni.pageScrollTo({
							    scrollTop: top - windowHeight + height + 10,
							    duration: 0
							});
							this.innerStyle.top = `${windowHeight - height - 10}px`;
						}
						//内容在可视区域以上
						if(top < 0){
							uni.pageScrollTo({
							    scrollTop: top,
							    duration: 0
							});
							this.nextGuide();
							return;
						}
						//如果在可视区域下方，提示文字显示在上方
						if(parseFloat(this.innerStyle.top) > windowHeight / 2){
							innerTextStyle.bottom = `${windowHeight - parseFloat(this.innerStyle.top) + 10}px`;
							delete innerTextStyle.top;
							this.nextBtnPosition = 'top';
						}else{
							this.nextBtnPosition = 'bottom';
						}
						this.innerTextStyle = innerTextStyle;
						this.innerText = dataset.guideText ?? '';
						
						this.guide++;
					} else {
						this.showGuide = false;
						uni.pageScrollTo({
						    scrollTop: 0,
						    duration: 0
						});
					}
				}).exec()
			},
			pxToRpx(val){
				return val/(uni.upx2px(100)/100);
			}
		}
	}
</script>

<style>
	.outer {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		background: transparent;
		overflow: hidden;
	}

	.inner {
		position: absolute;
		box-shadow: rgba(0, 0, 0, .8) 0 0 0 2021px;
		z-index: 99999;
	}

	.innerText {
		color: #fff;
		position: absolute;
		z-index: 99999;
		font-size: 28rpx;
		letter-spacing: 2rpx;
		text-align: center;
	}
	.next-btn-wrap{
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.next-btn{
		display: flex;
		justify-content: center;
		align-items: center;
		height: 60rpx;
		padding: 0 30rpx;
		border: 2rpx solid #ddd;
		border-radius: 10rpx;
		margin: 10rpx;
		font-size: 26rpx;
	}
</style>
