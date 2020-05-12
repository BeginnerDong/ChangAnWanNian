<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">题目解析</view>
			</view>
			
			<view class="answerList" v-for="(item,index) in answerData" :key="index">
				<view class="mx-3">
					<view>1、“先天下之忧而优，后天下之乐而乐”这是哪位诗人所写的？</view>
					<view class="answerBj px-3 d-flex a-start py-2 mt-2 ">
						<view class="color6 mr-5 ">答案</view>
						<view class="mr-2">范仲淹</view>
						<view class="answerImg"><image src="../../static/images/anti-img2.png" mode=""></image></view>
					</view>
					<view class="jiexiTit mt-3 font-weight">题目解析</view>
					<view class="color6 my-2">行政划分进口量胆识过人红素个人更健康粉红色客服号房会热哥斯拉回顾思考和狗肉火锅已五谷丰登沙鲁克汗个他如果和共同繁荣和</view>
				</view>
				<view class="f5Bj-H20 mb-3"><image src="../../static/images/home-icon4.png" mode=""></image></view>
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
				answerData:3
				
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
	page{padding-bottom: 100rpx;}
	.answerList:last-child .f5Bj-H20{display: none;}
	.answerBj{background-color: #f5e8d6;}
	.jiexiTit{position: relative;padding-left: 20rpx;}
	.jiexiTit::before{content: "";width: 8rpx;height: 24rpx;background-color: #D0B487;position: absolute;left: 0;top: 6rpx;}
	.answerImg{width: 150rpx;height: 150rpx;}
</style>
