<template>
	<view>
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">搜索穿越</view>
			</view>
			
			<view class="px-3">
				<view class="font-30 font-weight mt-1">唐长安城地图</view>
				<view class="map mt-2">
					<view class="mapInfor d-flex a-center">
						<!-- 宫殿左边 -->
						<view class="item" style="width: 260rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceLeft/palaceLeft'}})">
							<image src="../../static/images/explorel-img2.png" mode=""></image>
						</view>
						<!-- 宫殿中间 -->
						<view class="item" style="width: 184rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceMiddle/palaceMiddle'}})">
							<image src="../../static/images/explorel-img3.png" mode=""></image>
						</view>
						<!-- 宫殿右边 -->
						<view class="item" style="width: 246rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceRight/palaceRight'}})">
							<image src="../../static/images/explorel-img4.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="proRow prolist mt-4">
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
				productData:4
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack({});
			},
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
	@import "../../assets/style/proRow.css";
	
	page{padding-bottom: 40rpx;}	
	
	.swiper-box {height: 280rpx;box-sizing: border-box;border-radius: 10rpx;overflow: hidden;}
	/* .swiper-box swiper-item{width: 100%;box-sizing: border-box;} */
	
	/* 分类导航 */
	.indHome .item{flex: 1;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 40rpx;line-height: 36rpx; }
	.indHome .item image{width:111rpx;height:112rpx; margin: 0 auto 16rpx auto;}
	
	.mapInfor{width: 100%;height: 360rpx;}
	.mapInfor .item{overflow: hidden;height: 360rpx;}
</style>
