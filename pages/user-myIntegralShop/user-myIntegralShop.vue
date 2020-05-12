<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">积分商城</view>
			</view>
			
			<view class="integral mx-3">
				<view class="item rounded10 overflow-h position-relative mb-3 px-3 py-4" v-for="(item,index) in integralData" :key="index">
					<view class="position-absoluteXY"><image src="../../static/images/integral-malli-img.png" mode=""></image></view>
					<view class="infor d-flex a-center j-sb main-text-color">
						<view class="ll">
							<view class="font-40 font-weight">抵扣券10元</view>
							<view class="font-30 mt-4">积分：500可兑换</view>
						</view>
						<view class="rr font-30 text-center color2 font-weight" @click="exchangeShow">点击兑换</view>
					</view>
				</view>
				
			</view>
			
			<!-- 兑换弹框 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="exchangeShow rounded20 bg-white" v-show="is_exchangeShow">
				<view class="closebtn" @click="exchangeShow">×</view>
				<view class="d-flex a-center j-center">
					<view style="width:70rpx;height: 70rpx;"><image src="../../static/images/about-img2.png" mode=""></image></view>
				</view>
				<view class="d-flex j-center mt-5 border-bottom pb-4">
					<view class="munber font-34">500积分</view>
				</view>
				
				
				<view class="submitbtn pdtb15"  style="margin-top:80rpx;">
					<button class="btn" style="width: 100%;" type="button" @click="exchangeOk">
						<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
						<view class="btnTit">兑换</view>
					</button>
				</view>
				
				
				<view class="alertBox font-26 text-white p-3 rounded20 text-center d-flex flex-column a-center j-center" v-show="is_exchangeOk">
					<view class="closebtn text-white"  @click="exchangeOk">×</view>
					<view class="py-2">您的积分不够兑换该商品！</view>
					<view class="">兑换成功！</view>
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
				is_show:false,
				integralData:3,
				is_exchangeShow:false,
				is_exchangeOk:false
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
			exchangeShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_exchangeShow = !self.is_exchangeShow
			},
			exchangeOk(){
				const self = this;
				self.is_exchangeOk = !self.is_exchangeOk
			}
		}
	};
</script>

<style>
	.integral .item{width: 100%;height: 220rpx;}
	.integral .infor{position: relative;z-index: 2;}
	.integral .infor .ll{width: 68%;}
	.integral .infor .rr{width: 28%;}
	
	/* 弹框 */
	.exchangeShow{width:80%;height:580rpx;padding: 100rpx 50rpx 50rpx 50rpx;position: fixed;left: 50%;top: 50%;transform: translate(-50%,-50%);z-index: 50; }
	.alertBox{position: absolute;top: 50%;left: 50%;transform: translate(-50%,-50%);width: 400rpx;height: 200rpx; z-index: 55;background-color: rgba(0,0,0,0.5);}
</style>
