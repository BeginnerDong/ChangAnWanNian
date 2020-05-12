<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">结果</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 150rpx;">
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
			
			<view class="text-center font-34 font-weight mt-4">恭喜你，哆啦A梦，完成答题！</view>
			<view class="text-center font-26 py-5">
				<span class="red font-weight" style="font-size:74rpx;">40</span>分
			</view>
			
			<view class="mx-5 px-3 d-flex j-sb a-center font-28 main-text-color  text-center pt-2">
				<view class="TwoBtn position-relative" @click="Router.navigateTo({route:{path:'/pages/buildUp-Result-History/buildUp-Result-History'}})">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-icon10.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">查看历史解析</view>
				</view>
				<view class="TwoBtn position-relative" @click="Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-icon10.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">继续作答</view>
				</view>
			</view>
			
			<view class="submitbtn mt-4">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/buildUp-analysis/buildUp-analysis'}})">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">查看本轮解析</view>
				</button>
			</view>
			
			
			<!-- 达到上限弹框 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="exchangeShow rounded20 bg-white" v-show="is_UpperLimit">
				<view class="closebtn" @click="UpperLimitShow">×</view>
				<view class="text-center px-3 font-30" style="line-height: 50rpx;height: 280rpx;">今日您的回答次数已达到上限，所不能再继续作答，您可以看看其他的模板</view>
				
				
				<view class="submitbtn mt-3" >
					<button class="btn" style="width: 100%;" type="button" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
						<view class="btnBj"><image src="../../static/images/electricityl-icon1.png" mode=""></image></view>
						<view class="btnTit">去看看</view>
					</button>
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
				is_UpperLimit:false
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
	
	/* 弹框 */
	.exchangeShow{width:80%;height:550rpx;padding: 100rpx 50rpx 50rpx 50rpx;position: fixed;left: 50%;top: 50%;transform: translate(-50%,-50%);z-index: 50; }
	.alertBox{position: absolute;top: 50%;left: 50%;transform: translate(-50%,-50%);width: 400rpx;height: 200rpx; z-index: 55;background-color: rgba(0,0,0,0.5);}
</style>
