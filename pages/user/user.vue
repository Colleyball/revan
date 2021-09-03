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
						<view class="text-title m-left">输入手机号码登录</view>
						<view class="inputView">
							<input @input="GetTel" maxlength="25" name="Name" placeholder="手机号码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<button type="default" class="login" @click="Login()">登录</button>
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
							<input @input="GetTel" maxlength="25" name="Name" placeholder="手机号码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<!--view class="inputView">
							<input @input="GetPaw" password="true" maxlength="25" name="Name" placeholder="密码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view>
						<view class="inputView">
							<input @input="GetPaw" password="true" maxlength="25" name="Name" placeholder="再次确认密码"
								placeholderStyle="color:gray" style="height:80rpx;line-height:80rpx;width:100%"></input>
						</view-->
						<button type="default" class="login reg" @click="Register()">注册</button>
					</view>
				</view>
			</swiper-item>
		</swiper>
		<view class="user-cover" v-if="user_cover">
			<view class="user-name">HELLO,{{name}}</view>
			<view class="user-status">{{user_tip}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current: 0,
				show_cover: false,
				animationData: {},
				off: false,
				user_cover: false,
				name: '',
				user_tip: ''
			}
		},
		onLoad: function() {},
		onShow: function() {
			this.checkUserStatus()
		},
		methods: {
			changePages(e) {
				console.log(e.detail.current)
				this.current = e.detail.current
			},
			GetName(e) {
				console.log(e.detail.value)
				this.name = e.detail.value
			},
			GetTel(e) {
				console.log(e.detail.value)
				this.tel = e.detail.value
			},
			swichNav(e) {
				this.current = e.currentTarget.dataset.current
				console.log(e)
			},
			Register() {
				var that = this
				uni.request({
					url: "http://revan.game-win.cn/api/reg",
					header: {
						"content-type": "application/x-www-form-urlencoded"
					},
					method: "POST",
					data: {
						name: that.name,
						tel: that.tel
					},
					success: (res) => {
						console.log(res.data)
						if (res.data.data == 0) {
							uni.showModal({
								title: '提示',
								content: '手机号已存在',
								confirmText: "确认", // 确认按钮文字 
								showCancel: false, // 是否显示取消按钮，默认为 true
								confirmColor: '#f55850',
								cancelColor: '#39B54A',
								success: (res) => {
									if (res.confirm) {
										//that.gotoPage('/pages/index/index')
									} else {
										console.log('else', res)
									}
								}
							})
						} else {
							this.uid = res.data.data
							this.status = 1
							uni.setStorageSync('uid', res.data.data)
							uni.setStorageSync('status', 1)
							uni.showModal({
								title: '提示',
								content: '注册成功，请等待管理员审核',
								confirmText: "确认", // 确认按钮文字 
								showCancel: false, // 是否显示取消按钮，默认为 true
								confirmColor: '#f55850',
								cancelColor: '#39B54A',
								success: (res) => {
									if (res.confirm) {
										that.checkUserStatus()
									} else {
										console.log('else', res)
									}
								}
							})
						}

					}
				})
			},
			Login() {
				var that = this
				uni.request({
					url: "http://revan.game-win.cn/api/login",
					header: {
						"content-type": "application/x-www-form-urlencoded"
					},
					method: "POST",
					data: {
						tel: that.tel
					},
					success: (res) => {
						if (res.data.data == 0) {
							uni.showModal({
								title: '提示',
								content: '手机号码输入错误，请重试',
								confirmText: "确认", // 确认按钮文字 
								showCancel: false, // 是否显示取消按钮，默认为 true
								confirmColor: '#f55850'
							})
						} else {
							uni.setStorageSync('userinfo', res.data.data)
							uni.setStorageSync('uid', res.data.data.uid)
							var status = res.data.data.status
							that.name = res.data.data.name
							if (status == 1) {
								that.user_tip = '待认证'
							}
							if (status == 2) {
								that.user_tip = '已认证'
							}
							that.user_cover = 1
						}
						console.log(res.data)
					}
				})
			},
			checkUserStatus() {
				var that = this
				var uid = wx.getStorageSync('uid')
				if (!uid) {
					return 0
				} else {
					uni.request({
						url: "http://revan.game-win.cn/api/user",
						header: {
							"content-type": "application/x-www-form-urlencoded"
						},
						method: "POST",
						data: {
							uid: uid
						},
						success: (res) => {
							var status = res.data.data.status
							that.status = status
							uni.setStorageSync('userinfo', res.data.data)
							uni.setStorageSync('uid', res.data.data.uid)
							that.name = res.data.data.name
							if (status == 1) {
								that.user_tip = '待认证'
							}
							if (status == 2) {
								that.user_tip = '已认证'
							}
							that.user_cover = 1
						}
					})
				}
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
		background: linear-gradient(top, #f5d900, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.95));
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

	.user-cover {
		position: fixed;
		width: 100%;
		height: 100%;
		top: 0;
		background: linear-gradient(45deg, #fbffc6,#ffa0b2,#7ef5ff);
		z-index: 10;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.user-name {
		width: 100%;
		text-align: center;
		font-size: 2.3em;
		font-weight: bold;
		color: #333333;
		text-shadow: 0 0 30rpx #e2e2e2;
	}

	.user-status {
		font-size: 0.9em;
		color: #999999;
		margin: 20rpx 0 0 0;
	}
</style>
