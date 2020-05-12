<template>
	<view>
		
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">电商文创</view>
			</view>
			
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item rounded10 mb-3" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/Online-ProListDetail/Online-ProListDetail'}})">
					<view class="pic"><image src="../../static/images/electricityl-img.png" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2 font-30">考古学家利用激光揭露100公里玛雅石路之谜</view>
						<view class="price font-30 font-weight mt-2 d-flex a-center">
							<view class="priceIcon"><image src="../../static/images/about-img2.png" mode=""></image></view>
							<view>666.00</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
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
				uni.navigateBack({
				});
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}	
	.backBtn{width:20rpx;height: 35rpx;position: absolute;top: 66rpx;left: 20rpx; z-index: 10;}
</style>
