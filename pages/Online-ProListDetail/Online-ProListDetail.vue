<template>
	<view style="height: 100%;">

		<pageBj></pageBj>

		<view class="Box">

		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="backBtn" @click="navigateBack">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">详情</view>
		</view>
		<view class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="banner-box">
				<image class="slide-image" :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" />
			</view>

			<view class="mx-3 py-2">
				<view class="font-30 font-weight pb-2">{{mainData.title}}</view>
				<view class="price font-30 font-weight d-flex a-center">
					<view class="priceIcon">
						<image src="../../static/images/about-img2.png" mode=""></image>
					</view>
					<view>{{mainData.price}}</view>
				</view>
			</view>

			<view class="f5Bj-H20">
				<image src="../../static/images/home-icon4.png" mode=""></image>
			</view>

			<view class="px-3">
				<view class="py-3 xqInfor">
					<view class="font-30 font-weight pb-3">商品详情</view>
					<view class="cont fs14 text-center">
						<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="xqbotomBar center px-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" @click="Router.back(1)">
					<view class="icon">
						<image src="../../static/images/electricityl-icon.png" mode=""></image>
					</view>
					<view class="mt-1">返回</view>
				</view>
			</view>
			<button class="d-flex fs12" open-type="contact" >
				<view class="ite flexColumn" @click="Router.back(1)">
					<view class="icon">
						<image src="../../static/images/kefu.png" mode=""></image>
					</view>
					<view class="mt-1">客服</view>
				</view>
			</button>
			<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30">
				<view class="w-50 text-center payBtn d-flex j-center a-center" style="width: 512rpx;" @click="Utils.stopMultiClick(goBuy)">
					<view class="payBtnBj">
						<image src="../../static/images/electricityl-icon1.png" mode=""></image>
					</view>
					<view class="position-relative main-text-color" style="z-index: 2;">立即购买</view>
				</view>
			</view>
		</view>
		
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
				Utils: this.$Utils,
				mainData: {},
				statusBar: app.globalData.statusBar
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;

			console.log('options', options)
			self.$Utils.loadAll(['getMainData', 'getUserInfoData'], self);
		},

		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
			//self.getUserInfoData()
		},

		methods: {
			navigateBack() {
				uni.navigateBack({
					delta: 1
				});
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},

			goBuy() {
				const self = this;
				uni.setStorageSync('canClick', false);

				self.orderList.push({
					product_id: self.mainData.id,
					count: 1,
					type: 1,
					product: self.mainData
				}, );
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/orderConfim/orderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	button{border: 0;padding: 0;margin: 0;background: none;line-height: 1.5;}
	button:after{border: 0;}
	.pageBox{margin-bottom: 110rpx;}

	.banner-box {
		width: 100%;
		height: 400rpx;
		box-sizing: border-box;
		overflow: hidden;
	}
</style>
