<template>
	<view>
		
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">电商文创</view>
			</view>
		
			<view class="banner-box">
				<image class="slide-image" src="../../static/images/electricityl-img1.png" />
			</view>
		
			<view class="mx-3 py-2">
				<view class="font-30 font-weight pb-2">墨西哥牛油果8枚单果200g左右</view>
				<view class="price font-30 font-weight d-flex a-center">
					<view class="priceIcon"><image src="../../static/images/about-img2.png" mode=""></image></view>
					<view>666.00</view>
				</view>
			</view>
			
			<view class="f5Bj-H20">
				<image src="../../static/images/home-icon4.png" mode=""></image>
			</view>
			
			<view class="px-3">
				<view class="py-3 xqInfor">
					<view class="font-30 font-weight pb-3">商品详情</view>
					<view class="cont fs14 text-center">
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
						<view><image src="../../static/images/electricityl-img2.png" mode="widthFix"></image></view>
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
					</view>
				</view>
			</view>
		
			<view class="xqbotomBar center px-3">
				<view class="d-flex fs12">
					<view class="ite flexColumn" @click="navigateBack">
						<view class="icon"><image src="../../static/images/electricityl-icon.png" mode=""></image></view>
						<view class="mt-1">返回</view>
					</view>
				</view>
				<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30">
					<view class="w-50 text-center payBtn d-flex j-center a-center" style="width: 512rpx;" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">
						<view class="payBtnBj"><image src="../../static/images/electricityl-icon1.png" mode=""></image></view>
						<view class="position-relative main-text-color" style="z-index: 2;">立即购买</view>
					</view>
				</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
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
	
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.banner-box {width: 100%;height: 400rpx;box-sizing: border-box;overflow: hidden;}
	
</style>
