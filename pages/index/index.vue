<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/orderList/orderList'}})">
				<view class="ll flex"><image class="icon" src="../../static/images/home-icon1.png" mode=""></image>订单列表</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/comment/comment'}})">
				<view class="ll flex"><image class="icon" src="../../static/images/home-icon.png" mode=""></image>评价列表</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="scan">
				<view class="ll flex"><image class="icon" src="../../static/images/home-icon2.png" mode=""></image>扫一扫</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <view style="display: flex;">
			<input placeholder="输入订单id" v-model="willId"/>
			<view @click="getMainData">核销</view>
		</view> -->
		
		
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
				willId:''
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['wxJsSdk','getUserInfoData'], self);
		},
		
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			scan() {
				const self = this;
				self.$jweixin.scanQRCode({
					needResult: 1, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
					scanType: ["qrCode", "barCode"], // 可以指定扫二维码还是一维码，默认二者都有
					success: function(res) {
						var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
						self.willId = result;
						self.getMainData()
					}
				});
			},
			
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
						jsApiList: ['scanQRCode'] // 必填，需要使用的JS接口列表
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
			
			orderUpdate(id,type) {
				const self = this;
				
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					transport_status:2
				};
				postData.searchItem = {
					id:self.willId,
					user_type:0
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						self.willId = '';
						
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				if(self.willId==''){
					self.$Utils.showToast('请输入订单id','none');
					return
				};
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					id:self.willId,
					user_type:0,
					shop_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data.info.data.length>0) {
						self.orderUpdate()
						
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding: 30rpx 4%;}
	.item .ll .icon{width: 30rpx;height: 30rpx;display: block;margin-right: 20rpx;}
</style>
