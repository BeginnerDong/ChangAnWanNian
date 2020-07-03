<template>
	<view style="height: 100%;">
		<pageBj></pageBj>

		<view class="Box">

			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)">
					<image src="../../static/images/back-icon.png" mode=""></image>
				</view>
				<view class="headBj">
					<image src="../../static/images/head-img.png" mode=""></image>
				</view>
				<view class="tit">我的订单</view>
			</view>
			<scroll-view scroll-y="true" @scrolltolower="Bottom" class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
				<view class="topNavFix f5bj">
					<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
						<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
						<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待支付</view>
						<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">待配送</view>
						<view class="tt" :class="curr==4?'on':''" @click="changeCurr('4')">待收货</view>
						<view class="tt" :class="curr==5?'on':''" @click="changeCurr('5')">已完成</view>
					</view>
				</view>
				<view class="topNavH"></view>

				<view class="mx-3 mt-3">
					<view class="proRow ">
						<view class="item mb-3 bg-white" v-for="(item,index) in mainData" :key="index">
							<view class="priList">
								<view class="font-24 d-flex j-sb a-center mb-2">
									<view class="color9">交易时间：{{item.create_time}}</view>
									<view class="red" v-if="item.pay_status==0">待支付</view>
									<view class="red" v-if="item.pay_status==1&&item.transport_status==0">待配送</view>
									<view class="red" v-if="item.pay_status==1&&item.transport_status==1">待收货</view>
									<view class="red" v-if="item.pay_status==1&&item.transport_status==2">已完成</view>
								</view>
								<view class="d-flex a-center j-sb">
									<view class="pic">
										<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''"
										 mode=""></image>
									</view>
									<view class="infor">
										<view class="tit avoidOverflow2">{{item.title}}</view>
										<view class="B-price d-flex a-center j-sb">
											<view class="price font-30 font-weight mt-2 d-flex a-center">
												<view class="priceIcon">
													<image src="../../static/images/about-img2.png" mode=""></image>
												</view>
												<view>{{item.price}}</view>
											</view>
											<view class="font-26">×{{item.count}}</view>
										</view>
									</view>
								</view>
							</view>
							<view class="underBtn d-flex j-end a-center pb-3" v-if="item.pay_status==0">
								<view class="Bbtn position-relative" @click="pay(index)">
									<view class="position-absoluteXY">
										<image src="../../static/images/about-icon.png" mode=""></image>
									</view>
									<view class="text">去支付</view>
								</view>
								<view class="Bbtn position-relative" @click="deleteThis(index)">
									<view class="position-absoluteXY">
										<image src="../../static/images/about-icon.png" mode=""></image>
									</view>
									<view class="text">取消订单</view>
								</view>
							</view>
							<view class="underBtn d-flex j-end a-center pb-3" @click="orderUpdate(index)" v-if="item.transport_status==1">
								<view class="Bbtn position-relative">
									<view class="position-absoluteXY">
										<image src="../../static/images/about-icon.png" mode=""></image>
									</view>
									<view class="text">确认收货</view>
								</view>
							</view>
						</view>
					</view>
				</view>


				<!-- 无数据 -->
				<view class="nodata" v-if="mainData.length==0">
					<image src="../../static/images/nodata.png" mode=""></image>
				</view>
			</scroll-view>
		</view>
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
				score: '',
				wx_info: {},
				is_show: false,
				curr: 1,
				orderData: 1,
				mainData: [],
				searchItem: {
					thirdapp_id: 2,
					//pay_status:1,
					type: 1
				},
				statusBar: app.globalData.statusBar
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},



		methods: {

			Bottom() {
				console.log('onReachBottom')
				const self = this;
				if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
					self.paginate.currentPage++;
					self.getMainData()
				};
			},

			pay(index) {
				const self = this;
				const postData = {};
				postData.wxPay = {
					price: parseFloat(self.mainData[index].price).toFixed(2)
				};
				postData.tokenFuncName = 'getProjectToken',
					postData.searchItem = {
						id: self.mainData[index].id
					};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {

										}
									});
									setTimeout(function() {

										self.getMainData(true)
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {

							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {

								}
							});
							setTimeout(function() {
								self.getMainData(true)
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},

			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					transport_status: 2,
				};
				postData.searchItem = {
					id: self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功', 'none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},

			deleteThis(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					status: -1,
				};
				postData.searchItem = {
					id: self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功', 'none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					orderItem: {
						tableName: 'OrderItem',
						middleKey: 'order_no',
						key: 'order_no',
						searchItem: {
							status: 1
						},
						condition: '='
					},
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

			changeCurr(curr) {
				const self = this;
				if (curr != self.curr) {
					self.curr = curr
					if (self.curr == 1) {
						delete self.searchItem.transport_status
						delete self.searchItem.pay_status
					} else if (self.curr == 2) {
						self.searchItem.pay_status = 0
						self.searchItem.transport_status = 0
					} else if (self.curr == 3) {
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
					} else if (self.curr == 4) {
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 1
					} else if (self.curr == 5) {
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";

	/* @import "../../assets/style/editInfor.css"; */
	page {
		background: #F5F5F5;
	}

	.topNavFix {
		width: 100%;
		height: 80rpx;
		position: fixed;
		right: 0;
		left: 0;
		box-sizing: border-box;
		z-index: 22;
	}

	.topNavH {
		height: 80rpx;
	}

	.proRow .item {
		padding: 0 30rpx;
	}

	.proRow .item .priList {
		padding: 30rpx 0;
		border-top: 1px solid #eee;
	}

	.proRow .item .priList:first-child {
		border-top: 0;
	}

	.orderNav .tt.on::after {
		bottom: 0;
	}
</style>
