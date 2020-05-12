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
						<view class="font-24 color6 mt-2"><span class="color2 font-weight mr-1" style="font-size: 50rpx;">壹</span>单选</view>
					</view>
				</view>
			</view>
			
			<view class="answerTit px-4 py-5 font-34 mt-2">
				<view>“先天下之忧而优，后天下之乐而乐”这是哪位诗人所写的？</view>
				<view class="d-flex j-end">
					<view class="zanBtn mr-1" @click="zan">
						<image v-show="!is_zan" src="../../static/images/explorel-icon1.png" mode=""></image>
						<image v-show="is_zan" src="../../static/images/explorel-icon2.png" mode=""></image>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pt-5">
				<button class="btn mb-3" type="button" >
					<!-- 错误× -->
					<view class="seltIconL errorIcon"><image src="../../static/images/anti-icon4.png" mode=""></image></view>
					
					<view class="btnBj"><image src="../../static/images/anti-icon1.png" mode=""></image></view>
					<view class="btnTit color2">杜牧</view>
				</button>
				<button class="btn mb-3" type="button">
					<!-- 正确 -->
					<view class="seltIconR rightIcon"><image src="../../static/images/anti-icon3.png" mode=""></image></view>
					<view class="btnBj"><image src="../../static/images/anti-icon2.png" mode=""></image></view>
					<view class="btnTit color2">范仲淹</view>
				</button>
			</view>
			
			<view class="submitbtn mt-5 pt-3">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/buildUp-Answer-multiSelt/buildUp-Answer-multiSelt'}})">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">下一题</view>
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
				answerData:["孟浩然","杜牧","范仲淹","李清照"]
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
	
</style>
