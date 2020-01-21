<template>
	<view>
		<view class="pdlr4 orderNav fs14 color9 flexRowBetween boxShaow">
			<view class="tt" :class="num==1?'on':''" @click="changeCurr('1')">待核销</view>
			<view class="tt" :class="num==2?'on':''" @click="changeCurr('2')">已核销</view>
		</view>
		<!-- <view class="f5H5"></view> -->
		
		<view class="orderList">
			<view class="item" v-for="(item,index) in mainData" :key="index">
				<view class="flexRowBetween fs13 pdb15">
					<view class="flex">
						<image class="userPhoto" :src="item.userInfo&&item.userInfo.mainImg&&item.userInfo.mainImg.length>0?item.userInfo.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						
						<view class="mgl10">{{item.userInfo?item.userInfo.name:''}}</view>
					</view>
					<view class="color6">{{item.create_time}}</view>
				</view>
				<view class="lisCont boxShaow">
					<view class="fxIcon" v-if="item.transport_status==2"><image src="../../static/images/the-order-icon.png" mode=""></image></view>
					<view class="fs10">{{item.title}}</view>
					<view class="fs16 pdt5 pdb10 ftw">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.description:''}}</view>
					<view class="xian"></view>
					<view class="flexRowBetween pdt10 fs11">
						<view>{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage1:''}}</view>
						<view class="fs10 color9">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?
							item.orderItem[0].snap_product.passage3:''}}</view>
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
				is_show:false,
				num:1,
				couponData:[{},{}],
				is_ewmShow:false,
				mainData:[],
				searchItem:{
					type:1,
					pay_status:1,
					transport_status:0,
					user_type:0
				},
				index:-1
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = uni.getStorageSync('user_info').user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeCurr(num){
				const self = this;
				if(num!= self.num){
					self.num = num
					if(self.num==1){
						self.searchItem.transport_status =0;
					}else if(self.num==2){
						self.searchItem.transport_status =2;
					}
					self.getMainData(true)
				}
			},
			
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
					userInfo:{
						tableName:'UserInfo',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['mainImg','name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
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
