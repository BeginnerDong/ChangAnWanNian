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
			<view class="tit">核销订单</view>
		</view>
		<scroll-view scroll-y="true" @scrolltolower="Bottom" class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="topNavFix f5bj">
				<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
					<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
					<view class="tt" :class="current==2?'on':''" @click="change('2')">待核销</view>
					<view class="tt" :class="current==3?'on':''" @click="change('3')">已核销</view>
				</view>
			</view>
			<view class="topNavH"></view>

			<view class="R-fixIcon" @click="scanCode">
				<image src="../../static/images/merchantsl-icon.png" mode=""></image>
			</view>
			<view class="mx-3 mt-3">
				<view class="proRow ">
					<view class="item mb-3 bg-white" v-for="(item,index) in mainData" :key="index">
						<view class="priList">
							<view class="font-24 d-flex j-sb a-center mb-2">
								<view class="color9">交易时间：{{item.create_time}}</view>
								<view class="red" v-if="item.transport_status==0">待核销</view>
								<view class="red" v-if="item.transport_status==2">已核销</view>
							</view>
							<view class="d-flex a-center j-sb">
								<view class="pic">
									<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product&&
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
						<view class="pb-3 d-flex a-center j-sb font-26 pt-3 border-top">
							<view class="color9">客户信息：</view>
							<view class="d-flex a-center j-end">
								<view>{{item.snap_address&&item.snap_address.name?item.snap_address.name:''}}</view>
								<view class="ml-3">{{item.snap_address&&item.snap_address.phone?item.snap_address.phone:''}}</view>
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


				mainData: [],
				current: 1,
				searchItem: {
					user_type: 0,
					thirdapp_id: 2,
					pay_status: 1,
					//level:1
					type: 2
				},
				statusBar: app.globalData.statusBar
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);

		},

		onShow() {
			const self = this;
			self.getMainData(true)
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
			
			scanCode() {
				const self = this;
				uni.scanCode({
					success: function(res) {
						self.getOrderData(res.result)
					}
				});
			},

			getOrderData(id) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.searchItem = {
					id: id,
					user_type: 0,
					//shop_no:uni.getStorageSync('merchant_info').user_no
				};

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.Router.navigateTo({
							route: {
								path: '/pages/user-heXiaoOrder-confim/user-heXiaoOrder-confim?id=' + res.info.data[0].id
							}
						})
					} else {
						self.$Utils.showToast('二维码无效')
					}
				};
				self.$apis.orderGet(postData, callback);
			},

			change(current) {
				const self = this;
				if (current != self.current) {
					self.current = current;
					if (self.current == 1) {
						delete self.searchItem.transport_status
					} else if (self.current == 2) {
						self.searchItem.transport_status = 0
					} else if (self.current == 3) {
						self.searchItem.transport_status = 2
					};
					self.getMainData(true)
				}
			},




			getMainData(isNew) {
				const self = this;
				console.log(2323)
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
				postData.tokenFuncName = 'getMerchantToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.getAfter = {
					orderItem: {
						tableName: 'OrderItem',
						middleKey: 'order_no',
						key: 'order_no',
						searchItem: {
							status: 1,
							user_type: 0
						},
						condition: '='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
						/* for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].totalCount = 0;
							for (var j = 0; j < self.mainData[i].child.length; j++) {
								self.mainData[i].totalCount += self.mainData[i].child[j].count
							}
						} */
					}
					//self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
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
