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
			<view class="tit">核销订单</view>
		</view>
		<scroll-view scroll-y="true" class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="proRow mx-3">
				<view class="item mb-3 bg-white">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{mainData.create_time}}</view>
							<view class="red" v-if="mainData.transport_status==0">待核销</view>
							<view class="red" v-if="mainData.transport_status==2">已核销</view>
						</view>
						<view class="d-flex a-center j-sb">
							<view class="pic">
								<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product&&
								mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''"
								 mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow2">{{mainData.title}}</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="price font-30 font-weight mt-2 d-flex a-center">
										<view class="priceIcon">
											<image src="../../static/images/about-img2.png" mode=""></image>
										</view>
										<view>{{mainData.price}}</view>
									</view>
									<view class="font-26">×{{mainData.count}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="pb-3 d-flex a-center j-sb font-26 pt-3 border-top">
						<view class="color9">客户信息：</view>
						<view class="d-flex a-center j-end">
							<view>{{mainData.snap_address&&mainData.snap_address.name?mainData.snap_address.name:''}}</view>
							<view class="ml-3">{{mainData.snap_address&&mainData.snap_address.phone?mainData.snap_address.phone:''}}</view>
						</view>
					</view>
				</view>
			</view>

			<view class="submitbtn pdtb15" style="margin-top:100rpx;">
				<button class="btn" type="button" v-if="mainData.transport_status==0" @click="orderUpdate(mainData.id)">
					<view class="btnBj">
						<image src="../../static/images/buttonl-icon.png" mode=""></image>
					</view>
					<view class="btnTit">确认</view>
				</button>
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
				Router: this.$Router,
				showView: false,
				score: '',
				wx_info: {},
				is_show: false,
				curr: 1,
				orderData: 1,
				is_hxEwmShow: false,
				mainData: {},
				statusBar: app.globalData.statusBar
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.searchItem = {
					id: self.id,
					user_type: 0
				};

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},

			orderUpdate(id) {
				const self = this;
				uni.setStorageSync('canClick', false);
				uni.showModal({
					title: '提示',
					content: '确定要核销此订单吗',
					confirmColor: '#db1b1b',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.tokenFuncName = 'getMerchantToken';
							postData.data = {
								transport_status: 2
							};
							postData.searchItem = {
								id: id,
								user_type: 0
							};
							const callback = (data) => {
								uni.setStorageSync('canClick', true);
								if (data && data.solely_code == 100000) {
									self.$Utils.showToast('操作成功', 'none');

									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										})
									}, 1000);
								} else {
									self.$Utils.showToast(data.msg, 'none')
								}
							};
							self.$apis.orderUpdate(postData, callback);
						} else if (res.cancel) {

						}
					}
				});

			},
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";

	page {
		background: #F5F5F5;
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
