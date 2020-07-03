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
			<view class="tit">课程订单</view>
		</view>
		<scroll-view scroll-y="true" @scrolltolower="Bottom" class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="topNavFix f5bj">
				<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
					<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
					<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待核销</view>
					<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已核销</view>
				</view>
			</view>
			<view class="topNavH"></view>

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
						<view class="pb-3 d-flex a-center j-sb font-26" v-if="item.transport_status==0">
							<view>核销码：</view>
							<view class="hxEwm" @click="hxEwmShow(index)">
								<image :src="item.qrcode" mode=""></image>
							</view>
						</view>
					</view>
				</view>
			</view>


			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0">
				<image src="../../static/images/nodata.png" mode=""></image>
			</view>

			<!-- 核销码放大弹框 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="hxEwmShow" v-show="is_hxEwmShow">
				<view>
					<image class="ewm" :src="mainData[chooseIndex].qrcode" mode=""></image>
				</view>
				<view class="closeBtn" @click="hxEwmShow">×</view>
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
				is_hxEwmShow: false,
				mainData: [],
				searchItem: {
					thirdapp_id: 2,
					pay_status: 1,
					type: 2
				},
				chooseIndex: -1,
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
					} else if (self.curr == 2) {
						self.searchItem.transport_status = 0
					} else if (self.curr == 3) {
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},

			hxEwmShow(index) {
				const self = this;

				if (index || index == 0) {
					self.chooseIndex = index
				};
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
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

	.hxEwm {
		width: 80rpx;
		height: 80rpx;
	}

	.hxEwmShow {
		width: 70%;
		position: fixed;
		top: 45%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 50;
	}

	.hxEwmShow .ewm {
		width: 420rpx;
		height: 420rpx;
		display: block;
		margin: 0 auto;
	}

	.closeBtn {
		background: rgba(0, 0, 0, 0.5);
	}
</style>
