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
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/scan/scan'}})">
				<view class="ll flex"><image class="icon" src="../../static/images/home-icon2.png" mode=""></image>扫一扫</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon3.png" mode=""></image>
				</view>
			</view>
		</view>
		<view style="display: flex;">
			<input placeholder="输入订单id" v-model="willId"/>
			<view @click="getMainData">核销</view>
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
				willId:''
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
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
