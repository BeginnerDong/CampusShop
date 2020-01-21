<template>
	<view>
		
		<view class="comment mglr4">
			<view class="child" v-for="(item,index) in mainData" :key="index">
				<view class="flexRowBetween">
					<view class="flex">
						<view class="photo">
							<image :src="item.mainImg&&item.mainImg.length>0?item.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
						</view>
						<view class="">
							<view class="fs12">{{item.title}}</view>
							<view class="fs10 color6">{{item.create_time}}</view>
						</view>
					</view>
					<view class="flexEnd">
						<view class="starBox mgr5">
							<view class="starBox mgr5">
								<image v-for="c_item in stars"
								:src="c_item<=item.score?'../../static/images/the-store-icon4.png':'../../static/images/the-store-icon5.png'" mode=""></image>
							</view>
						</view>
					</view>
				</view>
				<view class="font14 pdt10">{{item.description}}</view>
				<view class="imgbox" v-if="item.bannerImg&&item.bannerImg.length>0">
					<view class="img lisThree" v-if="item in bannerImg">
						<image :src="item.url" mode=""></image>
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
				commentData:[{},{},{}],
				mainData:[],
				stars:[1,2,3,4,5]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:4,
					relation_user:uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/star.css";
	@import "../../assets/style/comment.css";
	page{padding-bottom: 60rpx;}
</style>
