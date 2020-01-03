<template>
	<view>
		
		<view class="registerBox">
			<view class="fs18 ftw pdt30 pdb10">注册账号</view>
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon.png" mode=""></image></view>
					<view class="rr">
						<input type="text" maxlength="11" value="" placeholder="店铺名称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon1.png" mode=""></image></view>
					<view class="rr">
						<input type="text" value="" placeholder="地址">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon2.png" mode=""></image></view>
					<view class="rr">
						<input type="text" value="" placeholder="联系人名称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon3.png" mode=""></image></view>
					<view class="rr">
						<input type="number" maxlength="11" value="" placeholder="手机号码">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon4.png" mode=""></image></view>
					<view class="rr flex" style="align-items: flex-start;">
						<view>营业执照</view>
						<view class="upImg mgl10">
							<view class="lis" @click="chooseImage">
								<block v-if="imageSrc">
									<image :src="imageSrc"></image>
								</block>
								<block v-else>
									<view class="addBtn flexCenter">
										<image class="jia" src="../../static/images/registered-icon5.png" mode=""></image>
									</view>
								</block>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="Wbtn" type="button" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">提交</button>
			</view>
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				imageSrc: ''
			}
		},
		onUnload() {
			const self = this;
			self.imageSrc = '';
		},
		onLoad() {
			const self = this;
		},
		methods: {
			chooseImage(){
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album'],
					success: (res) => {
						console.log('chooseImage success, temp path is', res.tempFilePaths[0])
						var imageSrc = res.tempFilePaths[0]
						uni.uploadFile({
							url: 'https://unidemo.dcloud.net.cn/upload',
							filePath: imageSrc,
							fileType: 'image',
							name: 'data',
							success: (res) => {
								console.log('uploadImage success, res is:', res)
								uni.showToast({
									title: '上传成功',
									icon: 'success',
									duration: 1000
								})
								this.imageSrc = imageSrc
							},
							fail: (err) => {
								console.log('uploadImage fail', err);
								uni.showModal({
									content: err.errMsg,
									showCancel: false
								});
							}
						});
					},
					fail: (err) => {
						console.log('chooseImage fail', err)
					}
				})
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	
	.registerBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee; align-items: flex-start;}
	.items .item .icon{padding: 0 30rpx;width: 15%;box-sizing: border-box;}
	.items .item .icon image{width: 32rpx;height: 32rpx;display: block;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
	.items .item .rr{width: 85%;}
	
	.upImg .lis{width: 260rpx;height: 168rpx;overflow: hidden;background: #F5F5F5;margin-right: 20rpx;}
	.upImg .lis image{width: 100%;height: 100%;display: block;}
	.upImg .addBtn{width: 100%;height: 100%;}
	.upImg .addBtn .jia{width: 66rpx;height: 66rpx;}
</style>
