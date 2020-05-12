<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">答题</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 60rpx;">
				<view class="answerTime position-relative text-center">
					<view class="position-absoluteXY"><image src="../../static/images/anti-img.png" mode=""></image></view>
					<view class="infor pt-2">
						<view class="font-24 color6 mt-2"><span class="color2 font-weight mr-1" style="font-size: 50rpx;">伍</span>选词</view>
					</view>
				</view>
			</view>
			
			<view class="answerTit px-4 py-5 font-34 mt-2">
				<view>请将下面方格中的字组成一句话。</view>
				<view class="d-flex j-end">
					<view class="zanBtn mr-1" @click="zan">
						<image v-show="!is_zan" src="../../static/images/explorel-icon1.png" mode=""></image>
						<image v-show="is_zan" src="../../static/images/explorel-icon2.png" mode=""></image>
					</view>
				</view>
				<view class="pt-2 pb-1 font-30" style="border-bottom: 1px solid #999; vertical-align: middle;">
					<view class="d-inline-block">戎课望边色，思归多苦颜<image class="d-inline-block errorIcon mt ml-2" style="vertical-align: baseline;" src="../../static/images/anti-icon4.png" mode=""></image><image class="d-inline-block rightIcon mt ml-2" style="vertical-align: baseline;" src="../../static/images/anti-icon3.png" mode=""></image></view>
				</view>
			</view>
			
			<view class="textSplit mb-4 position-relative">
				<view class="position-absoluteXY"><image src="../../static/images/anti-img5.png" mode=""></image></view>
				<view class="cont font-40 d-flex a-start flex-wrap text-center font-weight">
					<view class="item" v-for="(item,index) in answerData" :key="index">{{item}}</view>
				</view>
			</view>
			
			
			<view class="submitbtn pt-5 mt-3" >
				<button class="btn mb-3" type="button" @click="Router.navigateTo({route:{path:'/pages/buildUp-Result/buildUp-Result'}})" >
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">完成</view>
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
				wx_info:{},
				is_zan:false,
				answerData:["戎","思","忘","边","望","尚","客","归","战","课","多","金","苦","颜","言","色"]
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
			},
			zan(){
				const self = this;
				self.is_zan = !self.is_zan
			}

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	.textSplit{width:635rpx;height: 658rpx;margin: 0 auto;padding: 24rpx 8rpx;}
	.textSplit .cont{position: relative;z-index: 2;}
	.textSplit .item{width: 25%;height: 156rpx;line-height: 156rpx;}
</style>
