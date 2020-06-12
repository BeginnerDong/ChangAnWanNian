<template>
	<view>
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">我的</view>
			</view>
			<view class="myHead text-center d-flex a-center flex-column">
				<view class="mny font-weight">{{yb}}</view>
				<view class="d-flex a-center font-26 mt-2"><image style="width:42rpx; height: 33rpx;margin-right: 10rpx;" src="../../static/images/yuanbao-img.png" mode=""></image>元宝(个)</view>
			</view>
			
			<view class="px-3 ">
				<view class="font-weight pt-3">充值金额 ({{yb_price}}元={{yb_num}}元宝)</view>
				<!-- <view class="input-money py-2" ><input type="number" value="100" placeholder="请输入金额" /></view>
				 -->
				<view class="specsList d-flex a-center text-center  pt-3" >
					<view class="item position-relative mb-2" :class="curr==index?'on':''" @click="changeCurr(index)" v-for="(item,index) in specsList" :key="index">
						<view class="position-absoluteXY"><image :src="curr==index?'../../static/images/top-upl-icon1.png':'../../static/images/top-upl-icon2.png'" mode=""></image></view>
						<view class="num font-30 color6 font-weight">{{item}}元</view>
					</view>
				</view>
			</view>
			
			<view class="px-3 pt-5">
				<view class="font-26 color6">支付方式</view>
				<view class="mt-2 d-flex a-center j-sb" @click="payChange('1')">
					<view class="d-flex a-center">
						<image style="width: 34rpx;height: 34rpx;" src="../../static/images/about-img2.png" mode=""></image>
						<view class="ml-1">余额支付</view>
					</view>
					<view class="seltIcon"><image :src="payCurr==1?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
				</view>
				<view class="mt-3 d-flex a-center j-sb" @click="payChange('2')">
					<view class="d-flex a-center">
						<image style="width: 34rpx;height: 34rpx;" src="../../static/images/yuanbao-icon.png" mode=""></image>
						<view class="ml-1">微信支付</view>
					</view>
					<view class="seltIcon"><image :src="payCurr==2?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></view>
				</view>
			</view>
			<view class="submitbtn pdtb15"  style="margin-top: 80rpx;">
				<button class="btn" type="button"  @click="addOrder()">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">立即充值</view>
				</button>
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
				showView: false,
				wx_info:{},
				is_show:false,
				specsList:[],
				curr:0,
				yb:0,
				price:0,
				payCurr:1,
				yb_price:0,
				yb_num:0
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				self.yb = parseFloat(parseFloat(uni.getStorageSync('user_info').info.yb_score) + parseFloat(uni.getStorageSync('user_info').info.yb_today)).toFixed(2);
				self.specsList = uni.getStorageSync('user_info').thirdApp.yb_price.split(',');
				self.price = parseFloat(self.specsList[self.curr]);
				self.yb_price = uni.getStorageSync('user_info').thirdApp.yb_price;
				self.yb_num = uni.getStorageSync('user_info').thirdApp.yb_num;
				uni.hideLoading();
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			payChange(payCurr){
				const self = this;
				if(payCurr!=self.payCurr){
					self.payCurr = payCurr
				}
			},
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price);
				var ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.charge_rate)/100;
				if(self.curr<0){
					self.$Utils.showToast('请选择购买数量', 'none');
					return
				};
				const postData = {};	
				postData.tokenFuncName = 'getProjectToken',
				postData.data = {
					price:parseFloat(self.price).toFixed(2),
					level:1,
					type:6
				}
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
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price);
				//var ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.charge_rate)/100;
				const postData = {};	
				if(self.payCurr==1){
					postData.balance = {
						price:parseFloat(self.price).toFixed(2),
					};
				}else{
					postData.wxPay = {
						price:parseFloat(self.price).toFixed(2),
					};
				}
				
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							type:6,
							count:self.yb_num,
							trade_info:'购买元宝',
							account:1,
							thirdapp_id:2,
							user_no:uni.getStorageSync('user_info').user_no
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('购买成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										});
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('购买失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('购买成功', 'none');
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
			
			changeCurr(index){
				const self = this;
				self.curr = index;
				self.price = parseFloat(self.specsList[self.curr]);
			},
			
			
		}
	};
</script>

<style>
	/* @import "../../assets/style/xieyiAlert.css"; */
page{padding-bottom: 60rpx;}	

.myHead{height: 300rpx;}
.myHead .mny{padding-top: 80rpx;font-size: 72rpx;line-height: 72rpx;}
.myRowBetween .item{padding: 20rpx 4%; border-bottom: 1px solid #eee;}

.input-money input{width: 55%;font-size: 30rpx;font-weight: bold;color: #FF2121; height: 60rpx;line-height: 60rpx;border-bottom: 1px solid #999999; display: block;}

.specsList{flex-wrap: wrap;}
.specsList .item{width: 214rpx;height: 101rpx; line-height: 100rpx;margin-right: 24rpx;}
.specsList .item:nth-child(3n){margin-right: 0;}
.specsList .item.on .num{color: #222;}
.specsList .item .num{z-index: 2;position: relative;}

.seltIcon{width: 38rpx;height: 38rpx;}
</style>

