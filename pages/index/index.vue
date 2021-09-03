<template>
	<view>
		<image class="logo" src="../../images/logo.png" mode="aspectFit"></image>
		<view class="list">
			<block v-for="(item,index) in main_section">
				<view class="main-section" @click="showSubSection(item.no, item.title)">
					<view class="main-section-no">{{index}}</view>
					<view class="main-section-content">
						<view class="main-section-title">{{item.title}}</view>
						<view class="main-section-update-time">{{item.time}}</view>
					</view>
				</view>
			</block>
		</view>
		<view class="sub-section-cover" v-if="show_subsection" :animation="animationData">
			<view class="sub-section-box">
				<view class="main-title">{{currentTitle}}</view>
				<view class="sub-section-list">
					<block v-for="(item,index) in sub_section">
						<view class="sub-section-content" @click="showArticleList(item.no,item.title)">
							<view class="sub-section-title">{{item.title}}</view>
							<view class="sub-section-update-time">{{item.time}}</view>
						</view>
						<view class="sub-section-line"></view>
					</block>
				</view>
			</view>
			<view class="sub-section-close" @click="closeSubSection">Close</view>
		</view>

		<view class="article-list-cover" v-if="show_article_list" :animation="animationData2">
			<view class="article-list-box">
				<view class="sub-title">{{currentSubTitle}}</view>
				<view class="article-list">
					<block v-for="(item, index) in article_list">
						<view class="article-content" @click="bindArticle(item.no)">
							<view class="file-type"></view>
							<view class="article-title">{{item.title}}</view>
						</view>
					</block>
				</view>
			</view>
			<view class="article-list-close" @click="closeArticleList">Back</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show_subsection: 0,
				animationData: {},
				animationData2: {},
				main_section: [],
				sub_section: [],
				article_list: [],
				currentSubTitle: '123',
				show_article_list: 0,
				status: 0
			}
		},
		onLoad() {

		},
		onShow() {
			this.getMainSection()
			this.checkUserStatus()
		},
		methods: {
			getMainSection() {
				uni.request({
					url: 'http://revan.game-win.cn/api/section',
					data: {},
					header: {
						'content-type': 'application/x-www-form-urlencoded'

					},
					success: (res) => {
						console.log(res.data)
						this.main_section = res.data.data
					}
				})
			},
			showSubSection(main_section_no, main_section_title) {
				if (this.checkUserClick() == 2) {
					console.log(main_section_title)
					console.log('subsection showed')
					this.currentTitle = main_section_title
					this.getSubSection(main_section_no)
					this.show_subsection = !this.show_subsection
					this.showCoverAnimation()
				}

			},
			getSubSection(no) {
				uni.request({
					url: 'http://revan.game-win.cn/api/subsection',
					data: {
						no: no
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'

					},
					success: (res) => {
						console.log(res.data.data)
						this.sub_section = res.data.data
					}
				})
			},
			closeSubSection() {
				var that = this
				this.hideCoverAnimation()
				this.sub_section  = []
				setTimeout(function() {
					console.log('subsection closed')
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
			showArticleList(sub_section_no, sub_section_title) {
				console.log(sub_section_title)
				console.log('article_list showed')
				this.currentSubTitle = sub_section_title
				this.getArticleList(sub_section_no)
				this.show_article_list = !this.show_article_list
				this.showArticleCoverAnimation()
			},
			getArticleList(no) {
				uni.request({
					url: 'http://revan.game-win.cn/api/article_list',
					data: {
						no: no
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'

					},
					success: (res) => {
						console.log(res.data.data)
						this.article_list = res.data.data
					}
				})
			},
			closeArticleList() {
				var that = this
				this.hideArticleCoverAnimation()
				this.article_list = []
				setTimeout(function() {
					console.log('article_list closed')
					that.show_article_list = !that.show_article_list
				}, 650);
			},
			showArticleCoverAnimation() {
				var animation = uni.createAnimation({
					duration: 600,
					timingFunction: 'ease',
				})
				this.animation = animation
				this.animation.translateX(-100).opacity(1).step()
				this.animationData2 = this.animation.export()
			},
			hideArticleCoverAnimation() {
				var animation = uni.createAnimation({
					duration: 600,
					timingFunction: 'ease',
				})
				this.animation = animation
				this.animation.translateX(100).opacity(0).step()
				this.animationData2 = this.animation.export()
			},
			bindArticle(no) {
				uni.navigateTo({
					url: '/pages/article/article?no=' + no,
				});
			},
			checkUserClick() {
				var status = this.status
				console.log (this.status)
				if (status == 0) {
					uni.showModal({
						title: '提示',
						content: '请注册或登录后继续',
						confirmText: "去登陆", // 确认按钮文字 
						showCancel: false, // 是否显示取消按钮，默认为 true
						confirmColor: '#f55850',
						cancelColor: '#39B54A',
						success: (res) => {
							if (res.confirm) {
								uni.switchTab({
									url: '/pages/user/user',
								})
							} else {
								console.log('else', res)
							}
						}
					})
					return 0
				}
				if (status == 1) {
					uni.showModal({
						title: '提示',
						content: '请等待管理员审核',
						confirmText: "好的", // 确认按钮文字 
						showCancel: false, // 是否显示取消按钮，默认为 true
						confirmColor: '#f55850'
					})
					return 1
				}
				return 2
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
							console.log(res.data.data.status)
							that.status = res.data.data.status
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


	.article-list-cover {
		opacity: 0;
		background: linear-gradient(top, #efefef, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.95));
		position: fixed;
		width: 90%;
		height: 100%;
		top: 000px;
		left: 100px;
		z-index: 32;
		padding: 300rpx 5% 0 5%;
	}

	.article-list-box {
		position: fixed;
		z-index: 33;
		width: calc(600rpx);
		left: 50rpx;
		display: flex;
		flex-direction: column;
		padding: 0 25rpx 25rpx 25rpx;
		border-radius: 40rpx;
		justify-content: space-between;
		margin: 20rpx 0;
	}

	.sub-title {
		width: calc(100% - 50rpx);
		line-height: 100rpx;
		border-radius: 40rpx;
		background: #efefef;
		padding: 0 25rpx;
		margin: 0 0 0 0rpx;
		box-shadow: 0 0 35rpx #e2e2e2;
	}

	.article-list {
		height: 100%;
		width: 100%;
		margin: 50rpx 0 0 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		padding: 0 0;
		box-sizing: border-box;
	}

	.article-content {
		width: 100%;
		height: 100rpx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin: 0 0 25rpx 0;
	}

	.file-type {
		width: 100rpx;
		height: 100rpx;
		background: white url(../../images/file-doc.png) no-repeat center;
		background-size: 40rpx;
		border-radius: 50rpx;
		box-shadow: 0 0 35rpx #e2e2e2;
	}

	.article-title {
		width: 460rpx;
		line-height: 100rpx;
		border-radius: 50rpx;
		background: white;
		text-align: center;
		color: #333333;
		box-shadow: 0 0 35rpx #e2e2e2;
	}

	.article-list-close {
		position: fixed;
		z-index: 33;
		background: #e2e2e2;
		box-shadow: 0 0 35rpx #e2e2e2;
		width: calc(600rpx - 50rpx);
		left: 75rpx;
		bottom: 425rpx;
		padding: 0 25rpx 0 25rpx;
		border-radius: 30rpx;
		height: 90rpx;
		line-height: 90rpx;
		text-align: center;
		color: #333333;
		font-size: 1.2em;
	}
</style>
