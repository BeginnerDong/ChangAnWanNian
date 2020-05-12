<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">会员卡</view>
			</view>
			<view class="px-3 mt-5">
				<view class="vipCard text-center position-relative">
					<view class="position-absoluteXY"><image src="../../static/images/vip-img.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">
						<view class="text-white font-weight" style="font-size: 52rpx; padding: 110rpx 0 100rpx 0;">月卡</view>
						<view class="d-flex j-center">
							<view class="price font-30 font-weight d-flex a-center">
								<view style="width: 36rpx;height: 36rpx;"><image src="../../static/images/about-img2.png" mode=""></image></view>
								<view class="ml-1 font-36">666</view>
							</view>
						</view>
					</view>
					
				</view>
				
				<view class="font-weight font-30 mt-2">会员权限：</view>
				<view class="xqInfor pt-1">
					<view class="cont font-26">
						<view>1、可以免费查看课程视频</view>
						<view>2、可以免费查看课程视频</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top: 80rpx;">
				<button class="btn" type="button">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">购买</view>
				</button>
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
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack({
				});
			}

		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 40rpx;}
	
	.vipCard{width: 521rpx;height: 688rpx;margin: 0 auto;}
</style>
