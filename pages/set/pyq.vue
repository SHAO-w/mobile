<template>
	<view class="container" style="padding-top: 0;">
		<!-- <scroll-view style="height: calc(100vh - 64px);" scroll-y="true"> -->
			<view class="album" v-for="(item,inx) in list" :key="inx">
				<view class="album__avatar">
					<image src="../../static/pd.jpg" mode="" style="width: 32px;height: 32px;"></image>
				</view>
				<view class="album__content">
					<u--text :text="'胖达'+item" type="primary" bold size="17"></u--text>
					<u--text margin="0 0 8px 0" text="拥有梅州飞机场的超级富婆,瓜皮不露脸网友!"></u--text>
					<u-album :urls="urls2"></u-album>
				</view>
			</view>
			<view  v-if="loading">
				<u-loading-icon text="加载中..." ></u-loading-icon>
			</view>
		<!-- </scroll-view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loading: false,
				list: [0,1,2,3],
				albumWidth: 0,
				urls2: [
					'https://cdn.uviewui.com/uview/album/1.jpg',
					'https://cdn.uviewui.com/uview/album/2.jpg',
					'https://cdn.uviewui.com/uview/album/3.jpg',
					'https://cdn.uviewui.com/uview/album/4.jpg',
					'https://cdn.uviewui.com/uview/album/5.jpg',
					'https://cdn.uviewui.com/uview/album/6.jpg',
					'https://cdn.uviewui.com/uview/album/7.jpg',
					'https://cdn.uviewui.com/uview/album/8.jpg',
					'https://cdn.uviewui.com/uview/album/9.jpg',
					'https://cdn.uviewui.com/uview/album/10.jpg',
				],
			}
		},
		onPullDownRefresh() {
			console.log('下拉刷新')
			setTimeout(() => {
				uni.stopPullDownRefresh()
			}, 1000)
		},
		onReachBottom() {
			console.log('下拉刷新')
			let that = this
			if (that.list.length !== 10) {
				that.loading = true
				setTimeout(() => {
					that.loading = false
					that.list.push(that.list.length)
				}, 1500)
			}
			
		},
		onLoad() {
			// uni.startPullDownRefresh()
		}
	}
</script>

<style lang="scss">
	.album {
		display: flex;
		align-items: flex-start;
		margin-top: 20px;
		margin-bottom: 20px;

		&__avatar {
			background-color: #eee;
			display: flex;
			padding: 5px;
			border-radius: 3px;
		}

		&__content {
			margin-left: 10px;
			flex: 1;

		}
	}
</style>
