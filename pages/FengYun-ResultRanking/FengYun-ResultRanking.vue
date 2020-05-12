<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">结果</view>
			</view>
			
			
			<view class="mx-3 rankingBox">
				<view class="item d-flex a-center j-sb py-3" v-for="(item,index) in rankingData" :key="index">
					<view class="ll d-flex a-center">
						<view class="num color9 font-weight">{{index+1}}</view>
						<view class="d-flex a-center">
							<view class="photo"><image src="../../static/images/the-results-ofl-img.png" mode=""></image></view>
							<view>
								<view class="font-26 color6 mb-1">哆啦A梦</view>
								<view class="">对战积分：400</view>
							</view>
						</view>
					</view>
					<view class="rr red text-right">本局积分：88</view>
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
				rankingData:5
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
	.rankingBox .item .num{width: 60rpx;height: 80rpx;margin-right: 20rpx;padding-top: 28rpx;line-height: 44rpx; font-size: 30rpx;text-align: center;background: url(../../static/images/the-results-ofl-icon3.png) no-repeat 0 0/100% 100%;}
	.rankingBox .item:first-child .num{background-image: url(../../static/images/the-results-ofl-icon.png);color: #e1b53e;}
	.rankingBox .item:nth-of-type(2) .num{background-image: url(../../static/images/the-results-ofl-icon1.png);color: #578aa3;}
	.rankingBox .item:nth-of-type(3) .num{background-image: url(../../static/images/the-results-ofl-icon2.png);color: #a05646;}
	
	.rankingBox .item .photo{width: 100rpx;height: 100rpx;border-radius: 50%;background-color: #E1E1E1;margin-right: 20rpx;}
	
	.rankingBox .ll{width: 68%;}
	.rankingBox .rr{width: 30%;}
</style>
