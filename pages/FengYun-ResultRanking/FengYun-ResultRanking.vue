<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">结果</view>
			</view>
			
		<scroll-view scroll-y="true" class="pageBox" :style="{top:statusBar + 'px'}">	
			<view class="mx-3 rankingBox">
				<view class="item d-flex a-center j-sb py-3" v-for="(item,index) in mainData" :key="index">
					<view class="ll d-flex a-center">
						<view class="num color9 font-weight">{{index+1}}</view>
						<view class="d-flex a-center">
							<view class="photo"><image :src="item.user&&item.user[0]?item.user[0].headImgUrl:''" mode=""></image></view>
							<view>
								<view class="font-26 color6 mb-1">{{item.user&&item.user[0]?item.user[0].nickname:''}}</view>
								<view class="">对战积分：{{item.user&&item.user[0]&&item.user[0].info?item.user[0].info.battle_score:''}}</view>
							</view>
						</view>
					</view>
					<view class="rr red text-right">本局积分：{{item.score}}</view>
				</view>
				
			</view>
			
			
			
		</scroll-view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj';
	const app = getApp();
	export default {
		components: {
			pageBj
		},
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				rankingData:5,
				mainData:[],
				statusBar:app.globalData.statusBar
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					set_id :uni.getStorageSync('setId'),
					//set_id:,
					user_type:0
				};
				postData.order = {
					score:'desc'
				};
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData,res.info.data)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.sheetGet(postData, callback);
			},

		},
	};
</script>

<style>
	page{padding-bottom: 100rpx;}
	.rankingBox .item .num{width: 60rpx;height: 80rpx;margin-right: 20rpx;padding-top: 28rpx;line-height: 44rpx; font-size: 30rpx;text-align: center;background: url(../../static/images/the-results-ofl-icon3.png) no-repeat 0 0/100% 100%;}
	.rankingBox .item:first-child .num{background-image: url(../../static/images/the-results-ofl-icon.png);color: #e1b53e;}
	.rankingBox .item:nth-of-type(2) .num{background-image: url(../../static/images/the-results-ofl-icon1.png);color: #578aa3;}
	.rankingBox .item:nth-of-type(3) .num{background-image: url(../../static/images/the-results-ofl-icon2.png);color: #a05646;}
	
	.rankingBox .item .photo{width: 100rpx;height: 100rpx;border-radius: 50%;background-color: #E1E1E1;margin-right: 20rpx;}
	
	.rankingBox .ll{width: 68%;}
	.rankingBox .rr{width: 30%;}
</style>
