<template>
	<view>
		<view class="swiper-tab">
			<view @click="swichNav" class="swiper-tab-list" :class="{on:current==0}" data-current="0">登录</view>
			<view @click="swichNav" class="swiper-tab-list" :class="{on:current==1}" data-current="1">注册</view>
		</view>
		<swiper class="swiper-box" :current="current" :autoplay="false" :interval="3000" :duration="1000"
			@change="changePages">
			<swiper-item>
				<view class="swiper-item">
					<view class="list">
						<view class="text-title m-left">输入用户名及密码</view>
						<view class="inputView">
							<input @input="GetName" maxlength="25" name="Name" placeholder="用户名"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<view class="inputView">
							<input @input="GetPaw" password="true" maxlength="25" name="Name" placeholder="密码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<button type="default" class="login">登录</button>
					</view>
				</view>
			</swiper-item>
			<swiper-item>
				<view class="swiper-item">
					<view class="list">
						<view class="text-title m-left">输入个人信息</view>
						<view class="inputView">
							<input @input="GetName" maxlength="25" name="Name" placeholder="用户名"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<view class="inputView">
							<input @input="GetPaw" password="true" maxlength="25" name="Name" placeholder="密码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<view class="inputView">
							<input @input="GetPaw" password="true" maxlength="25" name="Name" placeholder="再次确认密码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<button type="default" class="login reg">注册</button>
					</view>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current: 0,
				show_cover: false,
				animationData: {},
				off: false
			}
		},
		onLoad: function() {
		},
		onshow: function() {

		},
		methods: {
			changePages(e) {
				console.log(e.detail.current)
				this.current = e.detail.current
			},
			GetName(e) {
				console.log(e.detail.value)
			},
			swichNav(e) {
				this.current = e.currentTarget.dataset.current
				console.log(e)
			},
			Login() {
				uni.request({
					url: "",
					header: {
						"content-type": "application/x-www-form-urlencoded"
					},
					method: "POST",
					data: {
						matchid: e
					},
					success: (res) => {
						console.log(res.data)
					}
				})
			}
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

	.swiper-tab {
		width: 100%;
		height: 220rpx;
		text-align: left;
		line-height: 50rpx;
		z-index: 20;
		background: linear-gradient(0, rgba(240, 240, 240, 1), #f8f8f8);
		border-bottom: 0rpx solid rgba(0, 0, 0, 0.1)
	}

	.swiper-tab-list {
		line-height: 120rpx;
		font-size: 40rpx;
		display: inline-block;
		width: auto;
		color: rgba(0, 0, 0, 0.4);
		padding-left: 35rpx;
	}

	.on {
		color: rgba(0, 0, 0, 0.95);
		font-size: 70rpx;
		font-weight: bold;
	}

	.swiper-box {
		display: block;
		height: 400px;
		width: 100%;
		overflow: hidden;
		padding-top: 0rpx;
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

	.text-title {
		height: 60rpx;
		line-height: 60rpx;
		font-size: 55rpx;
		margin: 75rpx 0 20rpx 0;
		color: black;
		text-align: left;
		width: 100%;
	}

	.inputView {
		font-size: 40rpx;
		display: -webkit-flex;
		display: flex;
		width: 680rpx;
		height: 80rpx;
		line-height: 80rpx;
		flex-direction: row;
		background-color: whitesmoke;
		border-radius: 0rpx;
		margin: 0 0 30rpx 0;
		text-align: center;
		box-shadow: 0rpx 0rpx 15rpx rgba(0, 0, 0, 0)
	}

	.login {
		width: 200rpx;
		height: 200rpx;
		line-height: 200rpx;
		border-radius: 50%;
		text-align: center;
		color: #3b3b3b;
		font-size: 40rpx;
		margin: 50rpx 0 0 0;
		background: linear-gradient(top, #ffe200, #fff120);
	}

	.reg {
		color: #fff;
		background: linear-gradient(top, #1391ff, #2ad1ff);
	}

	.upload-cover {
		opacity: 0;
		background: linear-gradient(top,#f5d900,rgba(255,255,255,0.95),rgba(255,255,255,0.95));
		position: fixed;
		width: 90%;
		height: 100%;
		top: -100px;
		z-index: 30;
		padding: 300rpx 5% 0 5%;
	}

	.upload-title {
		height: 60rpx;
		line-height: 60rpx;
		font-size: 55rpx;
		margin: 75rpx 0 20rpx 0;
		color: black;
		text-align: left;
		width: 100%;
	}

	.upload-source {
		width: 100%;
		height: auto;
		display: flex;
		flex-direction: row;
	}

	.source-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0 30rpx 0 0;

	}

	.source-item-icon {
		width: 120rpx;
		height: 120rpx;
	}

	.icon-circle {
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
	}

	.source-item-text {
		line-height: 30rpx;
		font-size: 28rpx;
		margin: 10rpx 0 0 0;
	}

	.cover-close {
		position: fixed;
		bottom: 320rpx;
		width: 100rpx;
		height: 100rpx;
		left: 325rpx;
		border-radius: 50%;
		line-height: 100rpx;
		background: #f5d900;
		z-index: 31;
		text-align: center;
		font-size: 40rpx;
		font-weight: bold;
		box-shadow: 0 0 15rpx rgba(0, 0, 0, 0.5);
	}
</style>
