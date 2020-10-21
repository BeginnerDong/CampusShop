<template>
	<view>
		
		<view class="registerBox">
			<view class="fs18 ftw pdt30 pdb10">注册账号</view>
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon.png" mode=""></image></view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="店铺名称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon1.png" mode=""></image></view>
					<view class="rr">
						<input type="text" v-model="submitData.address" placeholder="地址">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon2.png" mode=""></image></view>
					<view class="rr">
						<input type="text" v-model="submitData.contacts" placeholder="联系人名称">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon3.png" mode=""></image></view>
					<view class="rr">
						<input type="number" maxlength="11" v-model="submitData.phone" placeholder="手机号码">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon3.png" mode=""></image></view>
					<view class="rr">
						<input type="text"  v-model="submitData.password" placeholder="密码">
					</view>
				</view>
				<view class="item flex borderB1">
					<view class="icon"><image src="../../static/images/registered-icon4.png" mode=""></image></view>
					<view class="rr flex" style="align-items: flex-start;">
						<view>营业执照</view>
						<view class="upImg mgl10">
							<view class="lis" @click="upLoadImg('licenseImg')">
								<block v-if="submitData.licenseImg&&submitData.licenseImg.length>0">
									<image :src="submitData.licenseImg&&submitData.licenseImg[0]?submitData.licenseImg[0].url:''"></image>
								</block>
								<block @click="upLoadImg('licenseImg')" v-if="submitData.licenseImg&&submitData.licenseImg.length==0">
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
				<button class="Wbtn" type="button" @click="Utils.stopMultiClick(submit)">提交</button>
			</view>
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					phone:'',
					name:'',
					address:'',
					contacts:'',
					licenseImg:[],
					password:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.setStorageSync('canClick',true)
			self.$Utils.loadAll(['wxJsSdk'], self);
		},
		
		methods: {
			
			
			wxJsSdk() {
				const self = this;
				const postData = {
					thirdapp_id: 2,
					url: location.href.split('#')[0]
				};
				const callback = (res) => {
					self.$jweixin.config({
						debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
						appId: res.appId, // 必填，公众号的唯一标识
						timestamp: res.timestamp, // 必填，生成签名的时间戳
						nonceStr: res.nonceStr, // 必填，生成签名的随机串
						signature: res.signature, // 必填，签名
						jsApiList: ['chooseImage',  
									'previewImage',  
									'uploadImage',  
									'downloadImage',] // 必填，需要使用的JS接口列表
					});
					self.$jweixin.ready(function() { //需在用户可能点击分享按钮前就先调用
					});
					self.$jweixin.error(function(res) {
						console.log('error', res)
					});
					self.$Utils.finishFunc('wxJsSdk');
				};
				self.$apis.WxJssdk(postData, callback);
			},
			
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						uni.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								thirdapp_id:2
							}, callback)
						}
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick',false)
				const postData = {
					data:self.$Utils.cloneForm(self.submitData)					
				}
				/* postData.smsAuth = {						
					phone:self.submitData.phone,						
					code:self.submitData.smsCode,
				}; */
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.licenseImg;
				if (self.$Utils.checkComplete(newObject)) {						
					const callback = (res) => {
						uni.setStorageSync('canClick',true)
						if (res.solely_code == 100000) {
							self.$Utils.showToast(res.msg, 'none');	
							/* uni.setStorageSync('user_token', res.token);
							uni.setStorageSync('user_info', res.info); */
							setTimeout(function() {
								self.Router.redirectTo({route:{path:'/pages/login/login'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					}
					self.$apis.registerShop(postData, callback);
				} else {
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请补全所需信息', 'none');
				};
			},
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
