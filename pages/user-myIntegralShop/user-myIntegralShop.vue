<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">积分商城</view>
			</view>
			
			<view class="integral mx-3">
				<view class="item rounded10 overflow-h position-relative mb-3 px-3 py-4" v-for="(item,index) in mainData" :key="index">
					<view class="position-absoluteXY"><image src="../../static/images/integral-malli-img.png" mode=""></image></view>
					<view class="infor d-flex a-center j-sb main-text-color">
						<view class="ll">
							<view class="font-40 font-weight">抵扣券{{item.value}}元</view>
							<view class="font-30 mt-4">积分：{{item.price}}可兑换</view>
						</view>
						<view class="rr font-30 text-center color2 font-weight" @click="exchangeShow(index)">点击兑换</view>
					</view>
				</view>
			</view>
			
			<!-- 兑换弹框 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="exchangeShow rounded20 bg-white" v-show="is_exchangeShow">
				<view class="closebtn" @click="exchangeShow(-1)">×</view>
				<view class="d-flex a-center j-center">
					<view style="width:70rpx;height: 70rpx;"><image src="../../static/images/about-img2.png" mode=""></image></view>
				</view>
				<view class="d-flex j-center mt-5 border-bottom pb-4">
					<view class="munber font-34">{{mainData[chooseIndex]?mainData[chooseIndex].price:''}}积分</view>
				</view>
				
				
				<view class="submitbtn pdtb15"  style="margin-top:80rpx;">
					<button class="btn" style="width: 100%;" type="button" @click="couponAdd()">
						<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
						<view class="btnTit">兑换</view>
					</button>
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
				is_show:false,
				integralData:3,
				is_exchangeShow:false,
				mainData:[],
				chooseIndex:-1
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
			
			couponAdd() {
				const self = this;
				
				self.orderList = [
					{coupon_id:self.mainData[self.chooseIndex].id,count:1,type:self.mainData[self.chooseIndex].type}
				];
				const postData = {
					tokenFuncName: 'getProjectToken',
				};
				postData.couponList = self.$Utils.cloneForm(self.orderList);
				postData.pay = {
					score:{
						price:parseFloat(self.mainData[self.chooseIndex].price)
					}
				};
				console.log('postData', postData)
				const callback = (res) => {
					self.exchangeShow(-1);
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none');
						
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponBuy(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					};
					self.$Utils.finishFunc('getMainData');	
				};
				self.$apis.couponGet(postData, callback);
			},
			
			
			exchangeShow(index){
				const self = this;
				if(index>-1||index==0){
					self.is_show = true;
					self.is_exchangeShow = true
					
				}else if(index==-1){
					self.is_show = false;
					self.is_exchangeShow = false
				}
				self.chooseIndex = index;
			},
			
			
			
		}
	};
</script>

<style>
	.integral .item{width: 100%;height: 220rpx;}
	.integral .infor{position: relative;z-index: 2;}
	.integral .infor .ll{width: 68%;}
	.integral .infor .rr{width: 28%;}
	
	/* 弹框 */
	.exchangeShow{width:80%;height:580rpx;padding: 100rpx 50rpx 50rpx 50rpx;position: fixed;left: 50%;top: 50%;transform: translate(-50%,-50%);z-index: 50; }
	.alertBox{position: absolute;top: 50%;left: 50%;transform: translate(-50%,-50%);width: 400rpx;height: 200rpx; z-index: 55;background-color: rgba(0,0,0,0.5);}
</style>
