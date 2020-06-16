<template>
	<view>

		<pageBj></pageBj>


		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">我的</view>
		</view>
		<view class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="userHead">
				<view class="infor mx-3">
					<view class="d-flex a-center mt-3">
						<view class="photo" style="overflow: hidden;">
							<open-data type="userAvatarUrl"></open-data>
						</view>
						<view style="width: 70%;">
							<view class="d-flex a-center mt-1">
								<view class="font-32">
									<open-data type="userNickName"></open-data>
								</view>
								<view class="font-24 color9 ml-2">等级：{{userInfoData.levelName&&userInfoData.levelName.length>0?userInfoData.levelName[0].title:'无'}}</view>
							</view>

							<view class="d-flex a-center mt-1" v-show="isMember">
								<view class="priceIcon">
									<image src="../../static/images/about-img1.png" mode=""></image>
								</view>
								<view class="font-24 color9">会员到期时间：{{Utils.timeto(userInfoData.member_time*1000,'ymd')}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>

			<view class="mx-3">

				<view class="rounded10 mt-4 px-3 pt-4 pb-2 position-relative shadow-sm" style="height: 170rpx;" v-show="!isMember"
				 @click="Router.navigateTo({route:{path:'/pages/user-Vip/user-Vip'}})">
					<view class="position-absoluteXY">
						<image src="../../static/images/about-img3.jpg" mode=""></image>
					</view>
					<view class="position-relative" style="z-index: 2;color: #cb9b3e;">
						<view class="font-36 font-weight">购买会员，享受优惠</view>
						<view class="main-text-color font-24 mt-1">精彩答题等你来答</view>
					</view>
				</view>


				<view class="rounded10 bg-white mt-3 px-3 py-2 d-flex a-center j-sb  shadow-sm">
					<view class="d-flex a-center j-sb pr-3 leftChange position-relative" style="width: 50%;">
						<view>
							<view class="d-flex a-center">
								<view class="mr-1" style="width: 38rpx;height: 38rpx;">
									<image src="../../static/images/about-img2.png" mode=""></image>
								</view>
								<view>余额</view>
							</view>
							<view class="font-32 font-weight mt-2">{{userInfoData.balance?userInfoData.balance:''}}</view>
						</view>
						<view class="payBtn position-relative d-flex j-center a-center" style="width: 140rpx;height: 60rpx;" @click="Router.navigateTo({route:{path:'/pages/uesr-Recharge/uesr-Recharge'}})">
							<view class="payBtnBj">
								<image src="../../static/images/about-icon.png" mode=""></image>
							</view>
							<view class="position-relative main-text-color" style="z-index: 2;">充值</view>
						</view>
					</view>

					<view class="d-flex a-center j-sb  pl-3" style="width: 50%;">
						<view>
							<view class="d-flex a-center">
								<view class="mr-1" style="width: 42rpx;height: 33rpx;">
									<image src="../../static/images/yuanbao-img.png" mode=""></image>
								</view>
								<view>元宝</view>
							</view>
							<view class="font-32 font-weight mt-2">{{yb}}</view>
						</view>
						<view class="payBtn position-relative d-flex j-center a-center" style="width: 140rpx;height: 60rpx;" @click="Router.navigateTo({route:{path:'/pages/uesr-lngotRecharge/uesr-lngotRecharge'}})">
							<view class="payBtnBj">
								<image src="../../static/images/about-icon.png" mode=""></image>
							</view>
							<view class="position-relative main-text-color" style="z-index: 2;">充值</view>
						</view>
					</view>

				</view>

				<!-- 管理工具 -->
				<view class="userBox2 rounded10 bg-white mt-3 font-24 shadow-sm">
					<view class="d-flex j-sb a-center py-3  mx-3 border-bottom">
						<view class="font-30 font-weight">管理工具</view>
					</view>
					<view class="menu d-flex a-center flex-wrap pt-4 text-center">
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
							<view class="icon">
								<image src="../../static/images/about-icon1.png"></image>
							</view>
							<view>我的订单</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-CourseOrder/user-CourseOrder'}})">
							<view class="icon">
								<image src="../../static/images/about-icon2.png"></image>
							</view>
							<view>课程订单</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
							<view class="icon">
								<image src="../../static/images/about-icon3.png"></image>
							</view>
							<view>收货地址</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myIntegralShop/user-myIntegralShop'}})">
							<view class="icon">
								<image src="../../static/images/about-icon4.png"></image>
							</view>
							<view>积分商城</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myIntegral/user-myIntegral'}})">
							<view class="icon">
								<image src="../../static/images/about-icon5.png"></image>
							</view>
							<view>积分流水</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myCoupon/user-myCoupon'}})">
							<view class="icon">
								<image src="../../static/images/about-icon6.png"></image>
							</view>
							<view>优惠券</view>
						</view>
						<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-heXiaoLogin/user-heXiaoLogin'}})">
							<view class="icon">
								<image src="../../static/images/about-icon7.png"></image>
							</view>
							<view>核销员入口</view>
						</view>
					</view>
				</view>
			</view>

			<!--底部tab键-->
			<view class="navbar">
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar1.png" />
					</view>
					<view class="text">首页</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar2.png" />
					</view>
					<view class="text">关于我们</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar3-a.png" />
					</view>
					<view class="text this-text">我的</view>
				</view>
			</view>
			<!--底部tab键 end-->
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
				userInfoData: {},
				isMember: false,
				yb: 0,
				statusBar: app.globalData.statusBar
			}
		},

		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},

		onShow() {
			const self = this;
			self.getUserInfoData()
		},

		methods: {

			getUserInfoData() {
				var self = this;
				var nowTime = (new Date()).getTime() / 1000;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					levelName: {
						tableName: 'Level',
						middleKey: 'level',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						if (self.userInfoData.member_time > nowTime) {
							self.isMember = true
						}
						self.yb = parseFloat(parseFloat(self.userInfoData.yb_score) + parseFloat(self.userInfoData.yb_today)).toFixed(2)
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/navbar.css";

	page {
		padding-bottom: 140rpx;
		background: #F5F5F5;
	}

	.userHead {
		width: 100%;
		height: 130rpx;
	}

	.userHead .photo {
		width: 124rpx;
		height: 124rpx;
		border-radius: 50%;
		margin-right: 20rpx;
		border: 1px solid #cfcdcc;
	}

	.userBox2 {
		width: 100%;
		/* height: 264rpx; */
		position: relative;
		z-index: 1;
	}

	.userBox2 .item {
		width: 25%;
		margin-bottom: 46rpx;
	}

	.userBox2 .item .icon {
		width: 50rpx;
		height: 50rpx;
		display: block;
		margin: 0 auto;
		margin-bottom: 16rpx;
		position: relative;
	}

	.userBox2 .item .icon image {
		width: 100%;
		height: 100%;
	}

	.leftChange:after {
		content: '';
		width: 1px;
		height: 40rpx;
		background: #e1e1e1;
		position: absolute;
		right: 0;
		top: 50%;
		transform: translateY(-50%);
	}
</style>
