<template>
	<view style="height: 100%;">
		<pageBj></pageBj>

		<view class="Box">

		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">收货地址</view>
		</view>
		<scroll-view class="pageBox pb-4"  @scrolltolower="Bottom"  :style="{marginTop:44+statusBar + 'px'}">
			<view class="myaddress-lis bg-white rounded10 overflow-h mx-3 mb-3" v-for="(item,index) in mainData" :key="index">
				<view class="name" @click="choose(index)">{{item.name}}
					<view class="numb">{{item.phone}}</view>
				</view>
				<view class="adrs">{{item.city+item.detail}}</view>
				<view class="seltBox d-flex j-sb a-center">
					<view class="L" :data-id="item.id" @click="updateAddress($event.currentTarget.dataset.id)">
						<image class="icon" :src="item.isdefault==1?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'"
						 mode=""></image>
						默认地址
					</view>
					<view class="R font-24 color9 d-flex a-center">
						<view class="child  d-flex a-center mr-4" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd?id='+$event.currentTarget.dataset.id}})">
							<image src="../../static/images/addressl-icon.png" mode=""></image>编辑
						</view>
						<view class="child  d-flex a-center" :data-id="item.id" @click="deleteAddress($event.currentTarget.dataset.id)">
							<image src="../../static/images/addressl-icon1.png" mode=""></image>删除
						</view>
					</view>
				</view>
			</view>

			<view class="addAdrsBtn d-flex j-center a-center font-30" @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd'}})">
				<image class="mr-2" style="width: 30rpx;height: 30rpx;" src="../../static/images/addressl-icon2.png" mode=""></image>新增地址
			</view>
		</scroll-view>
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
				mainData: [],
				Router: this.$Router,
				choosedIndex: -1,
				statusBar: app.globalData.statusBar
			}
		},

		onShow(options) {
			const self = this;
			self.mainData = [];
			var res = self.$Token.getProjectToken(function() {
				self.$Utils.loadAll(['getMainData'], self)
			});
			if (res) {
				self.$Utils.loadAll(['getMainData'], self)
			};
		},

		

		methods: {
			
			Bottom() {
				console.log('onReachBottom')
				const self = this;
				if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
					self.paginate.currentPage++;
					self.getMainData()
				};
			},

			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta: 1
				})
			},

			getMainData() {
				const self = this;

				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},





			deleteAddress(id) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认是否删除这个地址',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.searchItem = {};
							postData.searchItem.id = id;
							postData.tokenFuncName = 'getProjectToken';
							const callback = (res) => {
								if (res) {
									self.mainData = [];
									self.getMainData();
								}
							};
							self.$apis.addressDelete(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},


			updateAddress(id) {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.data = {
					isdefault: 1
				}
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressUpdate(postData, callback);
			},

		}
	}
</script>

<style>
	/* @import "../../assets/style/seltAlert.css"; */
	page {
		background: #f5f5f5;
	}

	.myaddress-lis {
		padding: 30rpx 4%;
		background: #fff;
	}

	.myaddress-lis .name {
		font-size: 28rpx;
		color: #222;
	}

	.myaddress-lis .numb {
		margin-left: 30rpx;
		width: 300rpx;
		display: inline-block;
	}

	.myaddress-lis .adrs {
		font-size: 26rpx;
		color: #999;
		line-height: 40rpx;
		padding: 10rpx 0;
	}

	.seltBox {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-top: 10rpx;
	}

	.seltBox .L {
		display: flex;
		align-items: center;
		font-size: 24rpx;
		color: #999;
	}

	.seltBox .L .icon {
		width: 30rpx;
		height: 30rpx;
		display: inline-block;
		margin-right: 10rpx;
	}

	.seltBox .R image {
		width: 30rpx;
		height: 30rpx;
		display: block;
		margin-right: 8rpx;
	}

	.seltAlert {
		background: rgba(0, 0, 0, 0.5);
		position: fixed;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		z-index: 999;
	}

	.seltAlert .infor {
		width: 80%;
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #fff;
		box-sizing: border-box;
		padding: 50rpx 30rpx;
	}

	.seltAlert .infor .btnB {
		width: 60%;
		margin: 0 auto;
	}

	.seltAlert .infor .btnB view {
		width: 120rpx;
		line-height: 50rpx;
		height: 50rpx;
		border-radius: 30rpx;
		border: 2rpx solid #eee;
	}

	.seltAlert .infor .btnB view.on {
		background: #ddd;
		border-color: #ddd;
	}

	.addAdrsBtn {
		height: 100rpx;
		box-sizing: border-box;
		position: fixed;
		right: 0;
		bottom: 0;
		left: 0;
		border-top: 1px solid #999999;
	}
</style>
