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
					<view class="font-30 font-weight pb-3">详情介绍</view>
					<view class="cont fs14 text-center">
						<view style="position: relative;">
							<video style="width: 100%;z-index:0;position: relative;height: 450rpx;" controls="true" :src="item.url" id="myVideo"
							 v-for="(item,index) in mainData.videoImg"></video>
							<view v-show="!play" style="position: absolute;width: 100%;z-index: 1;top:0;left:0;height:450rpx;background-color: #000000;opacity: 0.5;"
							 @click="vedioPlay">
						
							</view>
						</view>
						<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar center px-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" @click="Utils.stopMultiClick(clickGood)">
					<!-- <view class="icon"><image src="../../static/images/electricityl-icon.png" mode=""></image></view> -->
					<view class="icon">
						<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/explorel-icon2.png':'../../static/images/explorel-icon1.png'"
						 mode=""></image>
					</view>
					<view class="mt-1">点赞</view>
				</view>
			</view>
			<view class="bottom-btnCont d-flex rounded50 overflow-h  font-30">
				<view class="w-50 text-center payBtn d-flex j-center a-center" style="width: 512rpx;" 
				@click="Router.navigateTo({route:{path:'/pages/user-Vip/user-Vip'}})">
					<view class="payBtnBj">
						<image src="../../static/images/electricityl-icon1.png" mode=""></image>
					</view>
					<view class="position-relative main-text-color">立即加入会员</view>
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
				isMember: false,
				statusBar: app.globalData.statusBar,
				play: false
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;

			console.log('options', options)
			self.$Utils.loadAll(['getMainData'], self);
		},

		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
			self.getUserInfoData()
		},

		methods: {

			

			vedioPlay() {
				const self = this;
				var videoContextPrev = wx.createVideoContext('myVideo')
				if (self.play) {
					videoContextPrev.pause();
					self.play = false
				} else {
					if (!self.isMember) {
						uni.showModal({
							title: '提示',
							content: '非会员暂无权限，是否立即购买会员？',
							showCancel: true,
							confirmText: '确定',
							success(res) {
								if (res.confirm) {
									self.Router.navigateTo({
										route: {
											path: '/pages/user-Vip/user-Vip'
										}
									})
								} else {
									console.log('取消')
								}
							}
						});
						//self.$Utils.showToast('非会员无权限', 'none', 1000)
						return
					}
					videoContextPrev.play();
					self.play = true;
				}
			},

			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (!self.isMember) {
					uni.setStorageSync('canClick', true);
					uni.showModal({
						title: '提示',
						content: '非会员暂无权限，是否立即购买会员？',
						showCancel: true,
						confirmText: '确定',
						success(res) {
							if (res.confirm) {
								self.Router.navigateTo({
									route: {
										path: '/pages/user-Vip/user-Vip'
									}
								})
							} else {
								console.log('取消')
							}
						}
					});
					return
				};
				//uni.setStorageSync('canClick', false);	
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},

			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.mainData.id,
					relation_table: 'Product',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});

						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('点赞失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.logAdd(postData, callback);
			},


			updateGoodLog() {
				const self = this;

				const postData = {
					searchItem: {
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
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
						path: '/pages/week-lecture-orderConfim/week-lecture-orderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},

			getUserInfoData() {
				const self = this;
				var nowTime = (new Date()).getTime() / 1000;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if (self.userInfoData.member_time > nowTime) {
							self.isMember = true
						}
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

			navigateBack() {
				uni.navigateBack({});
			},

			zan() {
				const self = this;
				self.is_zan = !self.is_zan
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Product'
						},
						condition: '='
					}
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
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";

	.pageBox{margin-bottom: 110rpx;}
	.banner-box {
		width: 100%;
		height: 400rpx;
		box-sizing: border-box;
		overflow: hidden;
	}

	/* .xqInfor{height: 200px;overflow: scroll;} */
</style>
