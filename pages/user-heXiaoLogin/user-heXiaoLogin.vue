<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">登录</view>
			</view>
			
			
			<view class="loginCont">
				<view class="item d-flex a-center px-2">
					<view class="icon"><image src="../../static/images/the-loginl-icon.png" mode=""></image></view>
					<view class="input">
						<input type="text" value="" placeholder="请输入账号" placeholder-class="placeholder">
					</view>
					<view class="xian"><image src="../../static/images/the-loginl-icon2.png" mode=""></image></view>
				</view>
				<view class="item d-flex a-center j-sb mt-5">
					<view class="d-flex a-center position-relative px-2" style="width: 66%;">
						<view class="icon"><image src="../../static/images/the-loginl-icon1.png" mode=""></image></view>
						<view class="input">
							<input type="password" value="" placeholder="请输入密码" placeholder-class="placeholder">
						</view>
						<view class="xian"><image src="../../static/images/the-loginl-icon2.png" mode=""></image></view>
					</view>
					<view class="border yzm main-text-color font-24 text-center rounded10">获取验证码</view>
				</view>
				
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top:100rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/user-heXiaoOrder/user-heXiaoOrder'}})">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">登录</view>
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
	
	page{padding-bottom: 40rpx;}
	
	.loginCont{margin: 0 auto;z-index: 2;padding: 0rpx 80rpx;box-sizing: border-box;padding-top:280rpx;}
	.loginCont .item{height: 70rpx;position: relative;}
	.loginCont .item .xian{position: absolute;left: 0;right: 0;bottom: 0;width: 100%;height: 20rpx;}
	.loginCont .item .icon{width: 36rpx;height: 36rpx;margin-right:20rpx;}
	.loginCont .item .input{width: 85%;}
	.loginCont .item .input input{font-size: 26rpx;height: 60rpx;}
	
	.yzm{width: 146rpx;height: 58rpx;line-height: 58rpx;border: 1px solid #d0b487;}
</style>
