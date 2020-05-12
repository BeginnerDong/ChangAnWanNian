<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">答题</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 80rpx;">
				<view class="answerTime position-relative text-center">
					<view class="position-absoluteXY"><image src="../../static/images/anti-img.png" mode=""></image></view>
					<view class="infor pt-2">
						<view class="font-24 color6"><span class="color2 font-weight mr-1" style="font-size: 50rpx;">肆</span>单选</view>
						<view class="font-26 mt">倒计时：<span class="red font-28">15秒</span></view>
					</view>
				</view>
			</view>
			
			<view class="answerTit px-4 py-5 font-34 mt-1">
				<view>在下列图中选择出属于唐代的瓦当？</view>
				<view class="d-flex j-end">
					<view class="zanBtn mr-1" @click="zan">
						<image v-show="!is_zan" src="../../static/images/explorel-icon1.png" mode=""></image>
						<image v-show="is_zan" src="../../static/images/explorel-icon2.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="mx-4">
				<view class="d-flex a-center flex-wrap seltImgBox">
					<view class="item mb-2" v-for="(item,index) in imgData" :key="index" @click="CurrChange(index)">
						<view class="cont d-flex flex-column a-center">
							<view class="img"><image src="../../static/images/anti-img2.png" mode=""></image></view>
							<view class="seltIcon mt-2"><image :src="curr==index?'../../static/images/anti-icon8.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
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
				wx_info:{},
				is_zan:false,
				imgData:4,
				curr:0
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
			},
			CurrChange(index){
				const self = this;
				self.curr = index;
				self.Router.navigateTo({route:{path:'/pages/buildUp-Answer-seltImg-Result/buildUp-Answer-seltImg-Result'}})
			}

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
</style>
