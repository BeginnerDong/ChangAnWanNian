<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">优惠券</view>
			</view>
			
			<view class=" tooling_indNav color6 mx-3">
				<view class="list rounded50 d-flex text-center j-sb bg-white shadow-sm">
					<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">未使用</view>
					<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">已使用</view>
				</view>
			</view>
			<view class="tooling_indNavH mb-3"></view>
			
			<view class="integral mx-3" v-if="curr==1">
				<view class="item rounded10 overflow-h position-relative mb-3 px-3 py-4" v-for="(item,index) in integralData" :key="index">
					<view class="position-absoluteXY"><image src="../../static/images/integral-malli-img.png" mode=""></image></view>
					<view class="infor d-flex a-center j-sb main-text-color">
						<view class="ll">
							<view class="font-40 font-weight">抵扣券10元</view>
							<view class="font-30 mt-4">积分：500可兑换</view>
						</view>
						<!-- <view class="rr font-30 text-center color2 font-weight"></view> -->
					</view>
				</view>
			</view>
			<view class="integral mx-3" v-if="curr==2">
				<view class="item rounded10 overflow-h position-relative mb-3 px-3 py-4" v-for="(item,index) in integralData" :key="index">
					<view class="position-absoluteXY"><image src="../../static/images/couponal-icon1.png" mode=""></image></view>
					<view class="infor d-flex a-center j-sb main-text-color">
						<view class="ll">
							<view class="font-40 font-weight">抵扣券10元</view>
							<view class="font-30 mt-4">积分：500可兑换</view>
						</view>
						<view class="rr d-flex j-center a-center">
							<view class="overdue"><image src="../../static/images/couponal-icon.png" mode=""></image></view>
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
				is_show:false,
				curr:1,
				integralData:2,
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
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	
	
	.tooling_indNav{position: fixed;top:72px;right: 0;left: 0;z-index: 12;}
	.tooling_indNavH{height: 70rpx;}
	
	.integral .item{width: 100%;height: 220rpx;}
	.integral .infor{position: relative;z-index: 2;}
	.integral .infor .ll{width: 68%;}
	.integral .infor .rr{width: 28%;}
	
	.overdue{width: 118rpx;height: 120rpx;}
</style>
