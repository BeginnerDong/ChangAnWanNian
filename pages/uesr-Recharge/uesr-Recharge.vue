<template>
	<view>
		<pageBj></pageBj>


		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">余额充值</view>
		</view>
		<scroll-view scroll-y="true" class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="myHead text-center d-flex a-center flex-column">
				<view class="mny font-weight">{{balance}}</view>
				<view class="d-flex a-center font-26 mt-2">
					<image style="width:36rpx; height: 36rpx;margin-right: 10rpx;" src="../../static/images/about-img2.png" mode=""></image>余额(元)
				</view>
			</view>

			<view class="px-3 ">
				<view class="font-weight pt-3">充值金额</view>
				<!-- <view class="input-money py-2" ><input type="number" value="100" placeholder="请输入金额" /></view>
				 -->
				<view class="specsList d-flex a-center text-center  pt-3">
					<view class="item position-relative mb-2" :class="curr==index?'on':''" @click="changeCurr(index)" v-for="(item,index) in specsList"
					 :key="index">
						<view class="position-absoluteXY">
							<image :src="curr==index?'../../static/images/top-upl-icon1.png':'../../static/images/top-upl-icon2.png'" mode=""></image>
						</view>
						<view class="num font-30 color6 font-weight">{{item}}元</view>
					</view>
				</view>
			</view>

			<view class="submitbtn pdtb15" style="margin-top: 80rpx;">
				<button class="btn" type="button" @click="addOrder()">
					<view class="btnBj">
						<image src="../../static/images/buttonl-icon.png" mode=""></image>
					</view>
					<view class="btnTit">立即充值</view>
				</button>
			</view>
			<view style="height: 260rpx;width: 100%;"></view>
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
				Router: this.$Router,
				showView: false,
				wx_info: {},
				is_show: false,
				specsList: [],
				curr: 0,
				balance: 0,
				price: 0,
				statusBar: app.globalData.statusBar
			}
		},

		onLoad(options) {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				self.balance = uni.getStorageSync('user_info').info.balance;
				self.specsList = uni.getStorageSync('user_info').thirdApp.charge_price.split(',');
				self.price = parseFloat(self.specsList[self.curr]);
				uni.hideLoading();
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			// self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.price = parseFloat(self.price);
				var ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.charge_rate) / 100;
				if (self.curr < 0) {
					self.$Utils.showToast('请选择充值金额', 'none');
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken',
					postData.data = {
						price: parseFloat(self.price * ratio).toFixed(2),
						level: 1,
						type: 7
					}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},


			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.price = parseFloat(self.price);
				var ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.charge_rate) / 100;
				const postData = {};
				postData.wxPay = {
					price: parseFloat(self.price * ratio).toFixed(2),
				};
				postData.tokenFuncName = 'getProjectToken',
					postData.searchItem = {
						id: self.orderId
					};
				postData.payAfter = [{
					tableName: 'FlowLog',
					FuncName: 'add',
					data: {
						type: 2,
						count: self.price,
						trade_info: '充值',
						account: 1,
						thirdapp_id: 2,
						user_no: uni.getStorageSync('user_info').user_no
					},
				}, ];
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

			changeCurr(index) {
				const self = this;
				self.curr = index;
				self.price = parseFloat(self.specsList[self.curr]);
			},


		}
	};
</script>

<style>
	/* @import "../../assets/style/xieyiAlert.css"; */
	page {
		padding-bottom: 60rpx;
	}

	.myHead {
		height: 300rpx;
	}

	.myHead .mny {
		padding-top: 80rpx;
		font-size: 72rpx;
		line-height: 72rpx;
	}

	.myRowBetween .item {
		padding: 20rpx 4%;
		border-bottom: 1px solid #eee;
	}

	.input-money input {
		width: 55%;
		font-size: 30rpx;
		font-weight: bold;
		color: #FF2121;
		height: 60rpx;
		line-height: 60rpx;
		border-bottom: 1px solid #999999;
		display: block;
	}

	.specsList {
		flex-wrap: wrap;
	}

	.specsList .item {
		width: 214rpx;
		height: 101rpx;
		line-height: 100rpx;
		margin-right: 24rpx;
	}

	.specsList .item:nth-child(3n) {
		margin-right: 0;
	}

	.specsList .item.on .num {
		color: #222;
	}

	.specsList .item .num {
		z-index: 2;
		position: relative;
	}
</style>
