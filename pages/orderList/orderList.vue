<template>
	<view>
		<view class="pdlr4 orderNav fs14 color9 flexRowBetween boxShaow">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">待核销</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">已核销</view>
		</view>
		<!-- <view class="f5H5"></view> -->
		
		<view class="orderList" v-show="curr==1">
			<view class="item" v-for="(item,index) in orderData" :key="index">
				<view class="flexRowBetween fs13 pdb15">
					<view class="flex">
						<image class="userPhoto" src="../../static/images/the-order-img1.png" mode=""></image>
						<view class="mgl10">用户名称</view>
					</view>
					<view class="color6">2019.12.25</view>
				</view>
				<view class="lisCont boxShaow">
					<view class="fs10">绿茶莉餐厅代金券</view>
					<view class="fs16 pdt5 pdb10 ftw">85元代100元券</view>
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>周一至周天可用</view>
						<view class="fs10 color9">10:00-22:00</view>
					</view>
				</view>
			</view>
		</view>
		<view class="orderList" v-show="curr==2">
			<view class="item" v-for="(item,index) in orderData" :key="index">
				<view class="flexRowBetween fs13 pdb15">
					<view class="flex">
						<image class="userPhoto" src="../../static/images/the-order-img1.png" mode=""></image>
						<view class="mgl10">用户名称</view>
					</view>
					<view class="color6">2019.12.25</view>
				</view>
				<view class="lisCont boxShaow">
					<view class="fxIcon"><image src="../../static/images/the-order-icon.png" mode=""></image></view>
					<view class="fs10">绿茶莉餐厅代金券</view>
					<view class="fs16 pdt5 pdb10 ftw">85元代100元券</view>
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>周一至周天可用</view>
						<view class="fs10 color9">10:00-22:00</view>
					</view>
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
				is_show:false,
				curr:1,
				orderData:[{},{}]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
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
	@import "../../assets/style/orderNav.css";
	.orderList .item{padding: 30rpx 4%;border-bottom: 10rpx solid #F5F5F5;}
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;}
	.lisCont{width: 100%;height: 240rpx;border-radius: 10rpx;background: url(../../static/images/the-order-img.png) 0 0/100% 100%;box-sizing: border-box;padding: 30rpx 40rpx;position: relative;}
	.lisCont .xian{border-bottom: 1px dashed #ff983d;}
	.fxIcon{width: 110rpx;height: 86rpx;position: absolute; top: 0;right:40rpx ;}
	.fxIcon image{width: 100%;height: 100%; display: block;}
</style>
