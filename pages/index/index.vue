<template>
	<view>
		<image class="logo" src="../../images/logo.png" mode="aspectFit"></image>
		<view class="list">
			<view class="main-section" @click="showSubSection">
				<view class="main-section-no">1</view>
				<view class="main-section-content">
					<view class="main-section-title">XXXXXX</view>
					<view class="main-section-update-time">2021-08-30</view>
				</view>
			</view>
			<view class="main-section">
				<view class="main-section-no">2</view>
				<view class="main-section-content">
					<view class="main-section-title">XXXXXX</view>
					<view class="main-section-update-time">2021-08-30</view>
				</view>
			</view>
		</view>
		<view class="sub-section-cover" v-if="show_subsection" :animation="animationData">
			<view class="sub-section-box">
				<view class="main-title">XXXXXX</view>
				<view class="sub-section-list">
					<view class="sub-section-content">
						<view class="sub-section-title">XXXXXX</view>
						<view class="sub-section-update-time">2021-08-30</view>
					</view>
					<view class="sub-section-line"></view>
					<view class="sub-section-content">
						<view class="sub-section-title">XXXXXX</view>
						<view class="sub-section-update-time">2021-08-30</view>
					</view>
				</view>
			</view>
			<view class="sub-section-close" @click="closeSubSection">Close</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show_subsection: 0,
				animationData: {}
			}
		},
		onLoad() {

		},
		methods: {
			showSubSection() {
				console.log ('subsection showed')
				this.show_subsection = !this.show_subsection
				this.showCoverAnimation()
			},
			closeSubSection() {
				var that = this
				this.hideCoverAnimation()
				setTimeout(function() {
					console.log ('subsection closed')
					that.show_subsection = !that.show_subsection
				}, 650);
			},
			showCoverAnimation() {
				var animation = uni.createAnimation({
					duration: 600,
					timingFunction: 'ease',
				})
				this.animation = animation
				this.animation.translateY(100).opacity(1).step()
				this.animationData = this.animation.export()
			},
			hideCoverAnimation() {
				var animation = uni.createAnimation({
					duration: 600,
					timingFunction: 'ease',
				})
				this.animation = animation
				this.animation.translateY(-100).opacity(0).step()
				this.animationData = this.animation.export()
			},
		}
	}
</script>

<style>
	page,
	.page {
		font-family: 'PingFang SC',
			'Helvetica Neue',
			Helvetica,
			'Droid Sans Fallback',
			'Microsoft Yahei',
			sans-serif;
		background: rgba(240, 240, 240, 1);
	}
	
	.logo {
		width: 680rpx;
		height: 100rpx;
		margin: 100rpx 35rpx;
	}

	.list {
		height: 100%;
		width: 680rpx;
		margin: 00rpx 0 0 35rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		padding: 0 0;
		box-sizing: border-box;
		font-family: "微软雅黑"
	}

	.main-section {
		width: calc(100% - 50rpx);
		display: flex;
		flex-direction: row;
		padding: 25rpx;
		border-radius: 20rpx;
		background: rgba(255, 255, 255, 1);
		justify-content: space-between;
		margin: 20rpx 0;
		box-shadow: 0 0 35rpx #e2e2e2;
	}

	.main-section-no {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		background: rgba(240, 240, 240, 1);
		text-align: center;
		line-height: 80rpx;
		font-size: 2em;
		font-weight: bold;
		color: white;
	}

	.main-section-content {
		width: 500rpx;
		display: flex;
		flex-direction: column;
		height: 80rpx;
		justify-content: space-between;
		color: #333333;
	}

	.main-section-title {
		line-height: 40rpx;
		font-size: 1.2em;
	}

	.main-section-update-time {
		line-height: 25rpx;
		text-align: right;
		font-size: 0.8em;
		color: #999999;
	}

	.sub-section-cover {
		opacity: 0;
		background: linear-gradient(top, #efefef, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.95));
		position: fixed;
		width: 90%;
		height: 100%;
		top: -200px;
		z-index: 30;
		padding: 300rpx 5% 0 5%;
	}

	.sub-section-box {
		position: fixed;
		z-index: 31;
		width: calc(600rpx - 50rpx);
		left: 75rpx;
		display: flex;
		flex-direction: column;
		padding: 0 25rpx 25rpx 25rpx;
		border-radius: 40rpx;
		background: rgba(255, 255, 255, 1);
		justify-content: space-between;
		margin: 20rpx 0;
		box-shadow: 0 0 35rpx #e2e2e2;
	}

	.main-title {
		width: 100%;
		line-height: 100rpx;
		border-radius: 40rpx 40rpx 0 0;
		background: #efefef;
		padding: 0 25rpx;
		margin: 0 0 0 -25rpx;
	}

	.sub-section-list {
		height: 100%;
		width: 100%;
		margin: 25rpx 0 0 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		padding: 0 0;
		box-sizing: border-box;
	}

	.sub-section-content {
		width: 100%;
		display: flex;
		flex-direction: column;
		height: 100rpx;
		justify-content: space-between;
		color: #333333;
	}

	.sub-section-title {
		line-height: 40rpx;
		font-size: 1.2em;
	}

	.sub-section-update-time {
		line-height: 25rpx;
		text-align: right;
		font-size: 0.8em;
		color: #999999;
	}

	.sub-section-line {
		width: 100%;
		height: 1rpx;
		border-bottom: 1rpx dashed #e2e2e2;
		margin: 15rpx 0;
	}

	.sub-section-close {
		position: fixed;
		z-index: 31;
		background: #e2e2e2;
		box-shadow: 0 0 35rpx #e2e2e2;
		width: calc(600rpx - 50rpx);
		left: 75rpx;
		bottom: 225rpx;
		padding: 0 25rpx 0 25rpx;
		border-radius: 30rpx;
		height: 90rpx;
		line-height: 90rpx;
		text-align: center;
		color: #333333;
		font-size: 1.2em;
	}
</style>
