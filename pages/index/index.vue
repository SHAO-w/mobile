<template>
	<view class="container">
		<view >
			<u-notice-bar :text="notice"></u-notice-bar>
		</view>
		<view class="margin10_t">
			<u-search :show-action="true" actionText="搜索" :animation="true"></u-search>
		</view>
		<view class="margin10_t">
			<u-swiper :list="imgList"></u-swiper>
		</view>
		<view class="margin10_t">
			<u-grid :border="false" col="4">
				<u-grid-item v-for="(listItem,listIndex) in iconlist" :key="listIndex" @click="iconClick(listItem)">
					<u-icon :customStyle="{paddingTop:20+'rpx'}" :name="listItem.name" :size="22"></u-icon>
					<text class="grid-text">{{listItem.title}}</text>
				</u-grid-item>
			</u-grid>
			<u-toast ref="uToast" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				iconlist: [{
						name: 'photo',
						title: '导航'
					},
					{
						name: 'lock',
						title: '个人中心'
					},
					{
						name: 'star',
						title: '朋友圈',
						path: '/pages/set/pyq'
					},
					{
						name: 'hourglass',
						title: '日记'
					},
					{
						name: 'home',
						title: '画廊',
						path: '/pages/set/three/index'
					},
					{
						name: 'star',
						title: '音乐',
						path: '/pages/set/music/index'
					},
					{
						name: 'setting',
						title: '设置',
						path: '/pages/set/set'
					}
				],
				imgList: [
					
				],
				notice: '从现在开始这里就是卢本伟广场！全体起立！'
			}
		},
		methods: {
			iconClick(item) {
				this.$refs.uToast.success(`点击了${item.path || item.name}`)
				uni.navigateTo({
					url: item.path
				})
			},
			getSwiperManage() {
				uni.request({
					url: 'http://sweet.com:8000/upload/upload/getSwiperImg2',
					method: 'POST',
					header: {
						'content-type': 'application/json'
					},
					success: (res) => {
						this.imgList = []
						res.data.data && res.data.data.forEach(el => {
							this.imgList.push(el.url)
						})
						
					}
				})
			}
		},
		onLoad() {
			this.getSwiperManage()
		}
	}
</script>

<style>
	
</style>
