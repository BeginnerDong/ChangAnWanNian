<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">购买</view>
			</view>
			
			
			<view class="px-5 mx-3 text-center" style="margin-top: 180rpx;">
				<view class="" style="height: 100rpx;">
					<view>查看历史答题解析需要购买</view>
					<view class="red mt-2">“10道题5元”</view>
				</view>
				<view class="font-44 font-weight py-5 my-2">长安万年平台</view>
				<view class="w-100" style="font-size: 70rpx; border-bottom: 2px solid #cacaca;">￥5</view>
				<view class="w-100 d-flex a-center j-sb py-3">
					<view class="color6">微信支付</view>
					<view class="seltIcon"><image src="../../static/images/anti-icon8.png" mode=""></image></view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages//'}})">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">支付</view>
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
	@import "../../assets/style/answer.css";
	.TwoBtn{width: 260rpx;height: 80rpx;line-height: 80rpx;}
</style>
