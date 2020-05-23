<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">积分流水</view>
			</view>
			<view class="myExtendTop text-center d-flex a-center flex-column  px-3">
				<view class="money font-weight">{{userInfoData.score}}</view>
				<view class="fs13 pdt10 d-flex j-center a-center mt-3"><image class="mr-1" style="width: 34rpx;height: 34rpx;" src="../../static/images/racel-icon1.png" mode=""></image>积分(个)</view>
			</view>
			
			<view class="f5Bj-H20">
				<image src="../../static/images/home-icon4.png" mode=""></image>
			</view>
			
			<view class="">
				<view class="myRowBetween mx-3" >
					<view class="item d-flex j-sb a-center border-bottom" v-for="(item,index) in mainData" :key="index">
						<view class="ll">
							<view class="">{{item.trade_info}}</view>
							<view class="font-26 color6 mt-1">{{item.create_time}}</view>
						</view>
						<view class="rr red">{{item.count}}</view>
					</view>
				</view>
				
			</view>
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj';
	export default {
		components: {
			pageBj
		},
		data() {
			return {
				Router:this.$Router,
				
				userInfoData:{},
				searchItem:{
					thirdapp_id:2,
					type:3,
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
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
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myExtendTop{padding-top:60rpx;height: 280rpx;}
	.myExtendTop .money{font-size:80rpx; line-height:80rpx;}
	.myRowBetween .item{border-bottom: 1px solid #E1E1E1;}
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
