<template>
	<view>
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">首页</view>
			</view>
			<view>
				<view class="banner-box px-3">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#d2d2d2" indicator-active-color="#dec193">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
			
			<view class="indHome d-flex a-center pt-4 font-24">
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/PassThrough/PassThrough'}})">
					<image src="../../static/images/home-icon.png"></image>
					<view class="tit">搜索穿越</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/buildUp/buildUp'}})">
					<image src="../../static/images/home-icon1.png"></image>
					<view class="tit">日积月累</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/FengYun/FengYun'}})">
					<image src="../../static/images/home-icon2.png"></image>
					<view class="tit">风云际会</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/week-lecture/week-lecture'}})">
					<image src="../../static/images/home-icon2.1.png"></image>
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
					<view class="item d-flex j-sb  rounded10 mb-4" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">
						<view class="pic"><image src="../../static/images/home-img.png" mode=""></image></view>
						<view class="infor">
							<view class="tit avoidOverflow2 font-28">考古学家利用激光揭露100公里玛雅石路之谜</view>
							<view class="B-price">
								<view class="font-24 color9">5月8日</view>
							</view>
						</view>
					</view>
				</view>
				
				<!-- 无数据 -->
				<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
				
			</view>
			
			<!--底部tab键-->
			<view class="navbar">
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar1-a.png" />
					</view>
					<view class="text this-text">首页</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar2.png" />
					</view>
					<view class="text">关于我们</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
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
	import pageBj from 'components/pageBj/pageBj'
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				productData:4
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proRow.css";
	
	page{padding-bottom: 140rpx;}	
	
	.swiper-box {height: 280rpx;box-sizing: border-box;border-radius: 10rpx;overflow: hidden;}
	/* .swiper-box swiper-item{width: 100%;box-sizing: border-box;} */
	
	/* 分类导航 */
	.indHome .item{flex: 1;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 40rpx;line-height: 36rpx; }
	.indHome .item image{width:111rpx;height:112rpx; margin: 0 auto 16rpx auto;}
	
</style>
