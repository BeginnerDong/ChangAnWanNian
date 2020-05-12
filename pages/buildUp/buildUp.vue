<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">日积月累</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 220rpx;">
				
				<view class="Dossier position-relative d-flex j-center a-center">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo"><image src="../../static/images/racel-img1.png" mode=""></image></view>
						<view class="rr ml-2">
							<view class="font-40 font-weight">汉塞时间</view>
							<view class="d-flex a-center mt-3 font-26">
								<view class="lableIcon mr-1"><image src="../../static/images/the-results-icon7.png" mode=""></image></view>
								<view class="">等级：学士</view>
							</view>
						</view>
					</view>
				</view>
				
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top:180rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">开始答题</view>
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
</style>
