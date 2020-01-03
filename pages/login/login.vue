<template>
	<view>
		
		<view class="loginBox">
			<view class="fs18 ftw pdt30 pdb10">登录</view>
			<view class="items fs13 color6">
				<view class="item flex borderB1">
					<input type="number" maxlength="11" value="" placeholder="手机号">
				</view>
				<view class="item flex borderB1 flexRowBetween">
					<view style="width: 50%;">
						<input type="text"  value="" placeholder="验证码">
					</view>
					<view class="fs14 ftw color2">获取验证码</view>
				</view>
			</view>
			<view class="pdt10 flex">
				<view class="fs13 color2" @click="Router.navigateTo({route:{path:'/pages/register/register'}})">没有账号，去注册</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="Wbtn" type="button" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">确定</button>
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
				is_show:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	
	.loginBox{padding: 0 10%;}
	.items .item{padding: 50rpx 0 20rpx 0;border-bottom: 1px solid #eee;}
	input{width: 100%; display: block;box-sizing: border-box;font-size: 26rpx;}
</style>
