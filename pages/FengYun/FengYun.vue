<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">风云际会</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 150rpx;">
				<view class="Dossier position-relative d-flex j-center a-center">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo"><image src="../../static/images/racel-img1.png" mode=""></image></view>
						<view class="rr ml-2">
							<view class="font-40 font-weight">汉塞时间</view>
							<view class="d-flex a-center mt-3 font-30">
								<view class="lableIcon mr-1"><image src="../../static/images/racel-icon1.png" mode=""></image></view>
								<view class="">积分：56</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="">
				<view style="width: 90rpx;height: 90rpx; margin: 50rpx auto;"><image src="../../static/images/racel-icon.png" mode=""></image></view>
			</view>
			
			<view class="d-flex j-center a-center">
				<view class="Dossier position-relative d-flex j-center a-center" @click="Router.navigateTo({route:{path:'/pages/FengYun-Result/FengYun-Result'}})">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo"><image src="../../static/images/racel-img.png" mode=""></image></view>
						<view class="rr ml-2">
							<view class="font-30 font-weight">点击邀请</view>
						</view>
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
	@import "../../assets/style/answer.css";
</style>
