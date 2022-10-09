<template>
	<view class="container">
		<u-toast ref="uToast"></u-toast>
		<u-upload accept="image"  class="upload" :fileList="imgList" @afterRead="afterReadImg"
			@delete="deleteImg" name="imgUp" multiple width="100%" :maxCount="9">
			<u-button type="primary" text="上传"></u-button>
		</u-upload>
		<u-modal :show="modal.show" @confirm="deleteConfirm" @cancel="modal.show = false" :showCancelButton="true">
			<view class="slot-content">
				<text>是否确认</text>
				<text>移除</text>
			</view>
		</u-modal>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgList: [],
				videoList: [],
				form: {
					title: ''
				},
				rules: {
					'title': {
						type: 'string',
						required: true,
						message: '请填写图片文字',
						trigger: ['blur', 'change']
					},
				},
				// 提示框数据
				modal: {
					show: false,
				},
				eventObj: {} // 删除图片的数据
			}
		},
		methods: {
			async afterReadImg(event) {
				console.log(event.file)
				let fileListLen = this.imgList.length
				event.file.map(item => {
					this.imgList.push({
						...item,
						status: 'uploading',
						message: '上传中'
					})
				})
				for (let i = 0; i < event.file.length; i++) {
					const result = await this.uploadFilePromise(event.file[i].url)
					console.log(result.data)
					let item = this.imgList[fileListLen] || {}
					if (result.data) {
						this.imgList.splice(fileListLen, 1, Object.assign(item, {
							status: 'success',
							message: '上传成功',
							url: result.data
						}))
						fileListLen++
					} else {
						this.imgList.splice(fileListLen, 1)
					}

				}
				console.log(this.imgList)
			},
			uploadFilePromise(url) {
				return new Promise((resolve, reject) => {
					let that = this
					let a = uni.uploadFile({
						url: 'http://sweet.com:8000/upload/upload/uploadSwiperImg', // 仅为示例，非真实的接口地址
						filePath: url,
						name: 'img',
						// formData: {
						// 	user: 'test'
						// },
						success: (res) => {
							that.$refs.uToast.show({
								type: 'success',
								message: JSON.parse(res.data).msg
							})
							setTimeout(() => {
								resolve(JSON.parse(res.data))
							}, 1000)
						}
					});
				})
			},
			deleteImg(event) {
				this.modal.show = true;
				this.eventObj = event
				console.log(this.eventObj)
			},
			deleteConfirm() {
				this.imgList.splice(this.eventObj.index, 1)
				this.modal.show = false;
				this.deletSwiperImg(this.eventObj.file.id)
			},
			getSwiperManage() {
				uni.request({
					url: 'http://sweet.com:8000/upload/upload/getSwiperImg2',
					method: 'post',
					success: (res) => {
						console.log(res)
						this.imgList = res.data.data || [];
					}
				})
			},
			deletSwiperImg(id) {
				let that = this
				uni.request({
					url: 'http://sweet.com:8000/upload/upload/deleteSwiperImg',
					data: {
						id,
					},
					success(res) {
						that.$refs.uToast.show({
							type: 'success',
							message: res.data.msg
						})
						console.log(res)
					}

				})
			}
		},
		mounted() {
			this.getSwiperManage()
		}
	}
</script>

<style lang="scss" scoped>
	/deep/.upload {
		.u-upload__wrap {
			flex-direction: column !important;
			flex-wrap: nowrap !important;
		}

		.u-upload__wrap__preview {
			margin: 0 0 14px 0;
		}
	}
</style>
