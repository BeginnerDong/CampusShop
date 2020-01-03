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
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding: 30rpx 4%;}
	.item .ll .icon{width: 30rpx;height: 30rpx;display: block;margin-right: 20rpx;}
</style>
