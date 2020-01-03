<template>
	<view>
		
		<view class="comment mglr4">
			<view class="child" v-for="(item,index) in commentData" :key="index">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo"><image src="../../static/images/the-order-img1.png" mode=""></image></view>
						<view class="">
							<view class="fs12">小白</view>
							<view class="fs10 color6">2019-12-25</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="starBox mgr5">
							<image src="../../static/images/the-store-icon4.png" mode=""></image>
							<image src="../../static/images/the-store-icon4.png" mode=""></image>
							<image src="../../static/images/the-store-icon4.png" mode=""></image>
							<image src="../../static/images/the-store-icon4.png" mode=""></image>
							<image src="../../static/images/the-store-icon5.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="font14 pdt10">真材细做，货真价实-信远斋桂花酸梅汤和是加客服的说法联合国就过来发的是两个号复健科单身公害了</view>
				<view class="imgbox">
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
					</view>
					<view class="img lisThree">
						<image src="../../static/images/the-store-img2.png" mode=""></image>
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
				commentData:[{},{},{}]
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
	@import "../../assets/style/star.css";
	@import "../../assets/style/comment.css";
	page{padding-bottom: 60rpx;}
</style>
