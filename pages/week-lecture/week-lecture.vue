<template>
	<view>
		
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">周末讲堂</view>
			</view>
			
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item mb-3" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/week-lecture-Detail/week-lecture-Detail'}})">
					<view class="pic rounded10"><image src="../../static/images/zhoumo-img.png" mode=""></image></view>
					<view class="infor avoidOverflow font-30">长安周末文化讲堂</view>
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
	
	.productList .item{height: auto;background-color: initial;border-radius: 0;}
	.productList .item .pic{height: 220rpx;overflow: hidden;}
	.productList .item .infor{padding: 10rpx 0 0 0;}
</style>
