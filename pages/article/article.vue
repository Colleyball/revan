<template>
	<view>
		<view class="title">{{article_title}}</view>
		<view class="time">{{article_time}}</view>
		<view class="content">
			<block v-for="(item, index) in article_img">
				<image :src="item" class="img" mode="widthFix"></image>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				no: '',
				article_title: '',
				article_time: '',
				article_img: []
			}
		},
		onLoad(option) {
			this.getArticleDetail(option.no)
		},
		onShow(option) {
		},
		methods: {
			getArticleDetail(no) {
				uni.request({
					url: 'http://revan.game-win.cn/article',
					data: {
						article_no: no
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						console.log(res.data.data)
						this.article_title = res.data.data.title
						this.article_time = res.data.data.time
						this.article_img = res.data.data.img_path
					}
				})
			},
		}
	}
</script>

<style>
	.title {
		width: auto;
		padding-left: 30rpx;
		border-left: 15rpx solid #ff5964;
		line-height: 55rpx;
		font-size: 1.1em;
	}
	
	.time {
		width: auto;
		padding-left: 45rpx;
		line-height: 35rpx;
		font-size: 0.9em;
		color: #999999;
		margin: 10rpx 0 0 0;
	}
	
	.content {
		width: 660rpx;
		display: flex;
		flex-direction: column;
		margin: 20rpx auto 0 auto;
		padding: 0 0 100rpx 0;
	}
	
	.img {
		width: 100%;
	}
</style>
