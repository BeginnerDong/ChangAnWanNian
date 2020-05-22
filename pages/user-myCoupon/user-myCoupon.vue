<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">优惠券</view>
			</view>
			
			<view class=" tooling_indNav color6 mx-3">
				<view class="list rounded50 d-flex text-center j-sb bg-white shadow-sm">
					<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">未使用</view>
					<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">已使用</view>
				</view>
			</view>
			<view class="tooling_indNavH mb-3"></view>
			
			<view class="integral mx-3">
				<view class="item rounded10 overflow-h position-relative mb-3 px-3 py-4" v-for="(item,index) in mainData" :key="index">
					<view class="position-absoluteXY" v-if="item.use_step==1"><image src="../../static/images/integral-malli-img.png" mode=""></image></view>
					<view class="position-absoluteXY" v-if="item.use_step==2"><image src="../../static/images/couponal-icon1.png" mode=""></image></view>
					<view class="infor d-flex a-center j-sb main-text-color">
						<view class="ll">
							<view class="font-40 font-weight">抵扣券{{item.value}}元</view>
							<!-- <view class="font-30 mt-4">积分：500可兑换</view> -->
						</view>
						<!-- <view class="rr font-30 text-center color2 font-weight"></view> -->
						<view class="rr d-flex j-center a-center" v-if="item.use_step==2">
							<view class="overdue"><image src="../../static/images/couponal-icon.png" mode=""></image></view>
						</view>
					</view>
				</view>
			</view>

			
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj'
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				integralData:2,
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					use_step:1,
					pay_status:1
				}
			}
		},
		onLoad(options) {
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
			
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					self.searchItem.use_step = self.curr;
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				var now =  (new Date()).getTime();
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	
	
	.tooling_indNav{position: fixed;top:72px;right: 0;left: 0;z-index: 12;}
	.tooling_indNavH{height: 70rpx;}
	
	.integral .item{width: 100%;height: 220rpx;}
	.integral .infor{position: relative;z-index: 2;}
	.integral .infor .ll{width: 68%;}
	.integral .infor .rr{width: 28%;}
	
	.overdue{width: 118rpx;height: 120rpx;}
</style>
