<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">

			<!-- <view  :style="{marginTop:statusBar + 'px'}"></view> -->
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
				<view class="headBj"></view>
				<view class="tit">首页</view>
			</view>
			<scroll-view scroll-y="true"  class="pageBox" :style="{marginTop:statusBar+44 + 'px'}">
				<view>
					<view class="banner-box px-3">
						<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="5000" duration="1000" indicator-color="#d2d2d2"
						 indicator-active-color="#dec193">
							<block v-for="(item,index) in sliderData" :key="index">
								<swiper-item class="swiper-item" :data-url="item.url" @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
									<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>

				<view class="indHome d-flex a-center pt-4 font-24">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/PassThrough/PassThrough'}})">
						<image src="../../static/images/home-icon.png"></image>
						<view class="tit">探索穿越</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/buildUp/buildUp'}})">
						<image src="../../static/images/home-icon1.png"></image>
						<view class="tit">日积月累</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/FengYun/FengYun'}})">
						<image src="../../static/images/home-icon2.1.png"></image>
						<view class="tit">风云际会</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/week-lecture/week-lecture'}})">
						<image src="../../static/images/home-icon2.png"></image>
						<view class="tit">周末讲堂</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/Online-ProList/Online-ProList'}})">
						<image src="../../static/images/home-icon3.png"></image>
						<view class="tit">电商文创</view>
					</view>
				</view>
				<view class="f5Bj-H20">
					<image src="../../static/images/home-icon4.png" mode=""></image>
				</view>

				<view class="px-3 mt-3">
					<view class="font-30 font-weight d-flex a-start title-xian mb-3">
						<view class="xain"></view>
						<view>热门文章</view>
					</view>
					<view class="proRow prolist">
						<view class="item d-flex j-sb  rounded10 mb-4" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
						 @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?id='+$event.currentTarget.dataset.id}})">
							<view class="pic">
								<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow2 font-28">{{item.title}}</view>
								<view class="B-price">
									<view class="font-24 color9">{{item.create_time}}</view>
								</view>
							</view>
						</view>
					</view>

					<!-- 无数据 -->
					<view class="nodata" v-if="mainData.length==0">
						<image src="../../static/images/nodata.png" mode=""></image>
					</view>

				</view>
			</scroll-view>


			<!--底部tab键-->
			<view class="navbar">
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar1-a.png" />
					</view>
					<view class="text this-text">首页</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar2.png" />
					</view>
					<view class="text">关于我们</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar3.png" />
					</view>
					<view class="text">我的</view>
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
				sliderData: {},
				mainData: [],
				statusBar: app.globalData.statusBar,
				customBar: app.globalData.customBar,
				custom: app.globalData.custom
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData', 'getSliderData', 'getMainData'], self);
		},

		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				
				return {
					title:'崇德含光文化平台',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title:'崇德含光文化平台',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					//imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}
		},

		onPageScroll: function(e) { //nvue暂不支持滚动监听，可用bindingx代替
			console.log("滚动距离为：" + e.scrollTop);
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

			getMainData() {
				const self = this;
				const postData = {};
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
				};
				postData.searchItem = {
					thirdapp_id: 2,
					is_show:1
				}
				postData.getBefore = {
					article: {
						tableName: 'Label',
						middleKey: 'menu_id',
						key: 'id',
						searchItem: {
							title: ['in', ['探索穿越']],
						},
						condition: 'in'
					}
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},


			getUserInfoData() {
				var self = this;
				var postData = {};
				var dayTime = new Date(new Date().toLocaleDateString()).getTime() / 1000;
				console.log('dayTime', dayTime);

				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						if (self.userInfoData.yb_daytime < dayTime) {
							/* if(self.userInfoData.yb_today>0){
								self.deleteFlowLog()
							} */
							self.addFlowLog()
						}
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: "首页轮播"
				};
				postData.getAfter = {
					child:{
						tableName:'Label',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'=',
						order:{
							listorder:'desc'
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0].child
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},

			addFlowLog() {
				const self = this;
				const postData = {};
				/* if(uni.getStorageSync('user_info').thirdApp.yuanbao<=parseFloat(self.userInfoData.yb_today)){
					return
				}; */
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					type: 7,
					count: uni.getStorageSync('user_info').thirdApp.yuanbao - parseFloat(self.userInfoData.yb_today),
					trade_info: '每日免费元宝',
					account: 1,
					thirdapp_id: 2,
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.saveAfter = [{
					tableName: 'UserInfo',
					FuncName: 'update',
					data: {
						yb_daytime: new Date(new Date().toLocaleDateString()).getTime() / 1000
					},
					searchItem: {
						user_no: uni.getStorageSync('user_info').user_no
					}
				}];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.showModal({
							title: '赠送',
							content: '恭喜获得系统每日免费赠送元宝，快去答题吧（每日清零）',
							showCancel: false,
							confirmText: '知道了',
							success(res) {
								if (res.confirm) {
									console.log('确定')
								} else {
									console.log('取消')
								}
							}
						})
					}
				};
				self.$apis.flowLogAdd(postData, callback);
			},


		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proRow.css";

	.pageBox {
		padding-bottom: 110rpx;
	}

	.swiper-box {
		height: 420rpx;
		box-sizing: border-box;
		border-radius: 10rpx;
		overflow: hidden;
	}

	/* .swiper-box swiper-item{width: 100%;box-sizing: border-box;} */

	/* 分类导航 */
	.indHome .item {
		flex: 1;
		text-align: center;
		color: #222;
		display: flex;
		flex-direction: column;
		margin-bottom: 40rpx;
		line-height: 36rpx;
	}

	.indHome .item image {
		width: 111rpx;
		height: 112rpx;
		margin: 0 auto 16rpx auto;
	}
</style>
