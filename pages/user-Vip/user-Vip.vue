<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">会员卡</view>
			</view>
			<view class="px-3 mt-5">
				<view class="vipCard text-center position-relative">
					<view class="position-absoluteXY"><image src="../../static/images/vip-img.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">
						<view class="text-white font-weight" style="font-size: 52rpx; padding: 110rpx 0 100rpx 0;">月卡</view>
						<view class="d-flex j-center">
							<view class="price font-30 font-weight d-flex a-center">
								<view style="width: 36rpx;height: 36rpx;"><image src="../../static/images/about-img2.png" mode=""></image></view>
								<view class="ml-1 font-36">{{price}}</view>
							</view>
						</view>
					</view>
					
				</view>
				
				<view class="font-weight font-30 mt-2">会员权益：</view>
				<view class="xqInfor pt-1">
					<view class="cont font-26">
						<view class="content ql-editor" style="padding:0;"
						v-html="mainData.content">
						</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top: 80rpx;">
				<button class="btn" type="button" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">购买</view>
				</button>
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
				Utils:this.$Utils,
				mainData:{},
				price:0
			}
		},
		
		onLoad() {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				self.price = uni.getStorageSync('user_info').thirdApp.member_price;
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				uni.showModal({
					title:'提示',
					content:'确定购买会员？',
					showCancel:true,
					confirmText:'继续',
					success(res) {
						if(res.confirm){
							const callback = (user, res) => {
								self.addOrder()
							};
							self.$Utils.getAuthSetting(callback);
						}else{
							console.log('取消')
						}
					}
				})
			},
			
			addOrder() {
				const self = this;
				self.price =  parseFloat(self.price);
				const postData = {};	
				postData.tokenFuncName = 'getProjectToken',
				
				postData.data = {
					price:self.price.toFixed(2),
					level:1,
					type:3
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					}else{
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
			
			
			pay() {
				const self = this;
				var nowTime = (new Date()).getTime() / 1000;
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price);
				const postData = {};	
				postData.score = {
					price:self.price.toFixed(2),
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.refreshToken = true;
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							member:1,
							member_time:nowTime+86400*30
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										});
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								});
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['会员权益']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 40rpx;}
	
	.vipCard{width: 521rpx;height: 688rpx;margin: 0 auto;}
</style>
