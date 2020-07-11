<template>
	<view v-if="showAll"  style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">
		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">登录</view>
		</view>

		<view class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="loginCont">
				<view class="item d-flex a-center px-2">
					<view class="icon">
						<image src="../../static/images/the-loginl-icon.png" mode=""></image>
					</view>
					<view class="input">
						<input type="text" v-model="submitData.login_name" placeholder="请输入账号" placeholder-class="placeholder">
					</view>
					<view class="xian">
						<image src="../../static/images/the-loginl-icon2.png" mode=""></image>
					</view>
				</view>
				<view class="item d-flex a-center px-2 mt-5">
					<view class="icon">
						<image src="../../static/images/the-loginl-icon1.png" mode=""></image>
					</view>
					<view class="input">
						<input type="password" v-model="submitData.password" placeholder="请输入密码" placeholder-class="placeholder">
					</view>
					<view class="xian">
						<image src="../../static/images/the-loginl-icon2.png" mode=""></image>
					</view>
				</view>
			</view>

			<view class="submitbtn pdtb15" style="margin-top:100rpx;">
				<button class="btn" type="button" @click="submit">
					<view class="btnBj">
						<image src="../../static/images/buttonl-icon.png" mode=""></image>
					</view>
					<view class="btnTit">登录</view>
				</button>
			</view>
		</view>
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj';
	const app = getApp();
	export default {
		components: {
			pageBj
		},
		data() {
			return {
				Router: this.$Router,

				submitData: {
					login_name: '',
					password: ''
				},
				showAll: false,
				statusBar: app.globalData.statusBar
			}
		},
		onLoad() {
			const self = this;
			uni.hideLoading();
			if (uni.getStorageSync('manager_token')) {
				uni.redirectTo({
					url: '/pages/user-mSubject/user-mSubject'
				})
			} else {
				self.showAll = true
			}
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {

			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
					password: self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('manager_token', res.token);
							uni.setStorageSync('manager_info', res.info);
							setTimeout(function() {
								uni.redirectTo({
									url: '/pages/user-mSubject/user-mSubject'
								})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg, 'none')
						}
					}
					self.$apis.login(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},

		},
	};
</script>

<style>
	.loginCont {
		margin: 0 auto;
		z-index: 2;
		padding: 0rpx 80rpx;
		box-sizing: border-box;
		padding-top: 280rpx;
	}

	.loginCont .item {
		height: 70rpx;
		position: relative;
	}

	.loginCont .item .xian {
		position: absolute;
		left: 0;
		right: 0;
		bottom: 0;
		width: 100%;
		height: 20rpx;
	}

	.loginCont .item .icon {
		width: 36rpx;
		height: 36rpx;
		margin-right: 20rpx;
	}

	.loginCont .item .input {
		width: 85%;
	}

	.loginCont .item .input input {
		font-size: 26rpx;
		height: 60rpx;
	}

	.yzm {
		width: 146rpx;
		height: 58rpx;
		line-height: 58rpx;
		border: 1px solid #d0b487;
	}
</style>
