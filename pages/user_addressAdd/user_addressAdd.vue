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
			<view class="tit">添加地址</view>
		</view>
		<view class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="myRowBetween mx-3 bg-white rounded10">
				<view class="item d-flex j-sb a-center">
					<view class="ll">姓名</view>
					<view class="rr fs13">
						<input type="text" v-model="submitData.name" placeholder="请写姓名" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view class="ll">手机号</view>
					<view class="rr fs13">
						<input type="number" v-model="submitData.phone" maxlength="11" placeholder="请写手机号" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view class="ll" style="width: 30%;">所在地区</view>
					
					<view class="rr fs13" style="width: 70%;">
						<picker mode="region" @change="cityChange" :class="submitData.city==''?'color9':'color6'">
							{{submitData.city!=''?submitData.city:'选择您的地区'}}
						</picker>
						<!-- <input type="text" placeholder="选择您的位置" disabled="true" v-model="submitData.city"> -->
						<image class="arrowR" src="../../static/images/the-orderl-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view class="ll">详细地址</view>
					<view class="rr fs13">
						<input type="text" v-model="submitData.detail" placeholder="请填写" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item d-flex j-sb a-center">
					<view class="ll">设为默认地址</view>
					<view class="rr">
						<switch style="transform:scale(0.75);" @change="choose" :checked="submitData.isdefault==1?true:false" color="#d0b487" />
					</view>
				</view>

			</view>
			<view class="submitbtn pdtb15" style="margin-top: 200rpx;">
				<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">
					<view class="btnBj">
						<image src="../../static/images/buttonl-icon.png" mode=""></image>
					</view>
					<view class="btnTit">新增</view>
				</button>
			</view>
		</view>
		</view>
	</view>
</template>

<script>
	import mpvuePicker from '../../components/mpvue-picker/mpvuePicker.vue';
	// https://github.com/zhetengbiji/mpvue-citypicker
	import mpvueCityPicker from '../../components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '../../common/city.data.js';
	const app = getApp();
	export default {
		components: {
			mpvuePicker,
			mpvueCityPicker
		},


		data() {
			return {
				Router: this.$Router,
				submitData: {
					name: '',
					city: '',
					detail: '',
					phone: '',
					isdefault: 0,
					latitude: '',
					longitude: ''
				},

				mulLinkageTwoPicker: cityData,
				cityPickerValueDefault: [0, 0, 0],
				themeColor: '#F98A48',
				Utils: this.$Utils,
				mode: '',
				deepLength: 1,
				pickerValueDefault: [0],
				pickerValueArray: [],
				statusBar: app.globalData.statusBar

			}
		},
		onLoad(options) {
			const self = this;
			if (options.id) {
				self.id = options.id;
				var res = self.$Token.getProjectToken(function() {
					self.$Utils.loadAll(['getMainData'], self)
				});
				if (res) {
					self.$Utils.loadAll(['getMainData'], self)
				};
			}
		},

		methods: {

			cityChange(e){
				const self = this;
				self.submitData.city = e.detail.value[0]+e.detail.value[1]+e.detail.value[2]
			},
				
			chooseAddress(e) {
				const self = this;
				uni.authorize({
					scope: 'scope.userLocation',
					success() {
						uni.chooseLocation({
							success: (res) => {
								console.log(res)
								self.submitData.city = res.name;
								self.submitData.longitude = res.longitude;
								self.submitData.latitude = res.latitude;
							},
							fail: (e) => {
								uni.getSetting({
									success: (res) => {
										console.log(res)
										let locaAuth = res.authSetting['scope.userLocation']
										if (locaAuth) { /* 判断位置是否已经授权，是选择地图位置点击取消触发的fail，再选择位置 */
											console.log('地图点击取消')
											uni.chooseLocation({
												success: (res) => {
													self.submitData.city = res.name;
													self.submitData.longitude = res.longitude;
													self.submitData.latitude = res.latitude;
												},
											});
										}
										if (!locaAuth) { /* 如果地理位置没授权 */
											console.log(222)
											uni.showModal({
												title: '提示',
												content: '需要授权位置信息',
												confirmColor: '#FC73AA',
												showCancel: true,
												success: function(res) {
													if (res.confirm) {
														uni.openSetting({
															success: (res) => {
																console.log(res.authSetting)
															},
															fail: (res) => {
																console.log(res)
															},
														});
													} else if (res.cancel) {

													}
												}
											});


										}
									}
								})
							}
						});
					},
					fail: (e) => {
						uni.showModal({
							title: '提示',
							content: '需要授权位置信息',
							confirmColor: '#FC73AA',
							showCancel: true,
							success: function(res) {
								if (res.confirm) {
									uni.openSetting({
										success: (res) => {
											console.log(res.authSetting)
										},
										fail: (res) => {
											console.log(res)
										},
									});
								} else if (res.cancel) {

								}
							}
						});
					}
				})

			},


			chooseLocation() {

				const self = this;
				uni.chooseLocation({
					success: function(res) {
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						self.submitData.detail = res.address;
						self.submitData.longitude = res.longitude;
						self.submitData.latitude = res.latitude;
					},
					fail() {
						uni.authorize({
							scope: 'scope.userLocation',
							success() {
								uni.chooseLocation({
									success: function(res) {
										console.log('位置名称：' + res.name);
										console.log('详细地址：' + res.address);
										console.log('纬度：' + res.latitude);
										console.log('经度：' + res.longitude);
										self.submitData.detail = res.address;
										self.submitData.longitude = res.longitude;
										self.submitData.latitude = res.latitude;

									},
								})
							}
						})
					}
				});
			},



			choose(e) {
				const self = this;
				if (e.target.value) {
					self.submitData.isdefault = 1
				} else {
					self.submitData.isdefault = 0
				}
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
			},

			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					console.log(res);
					self.submitData.name = res.info.data[0].name;
					self.submitData.city = res.info.data[0].city;
					self.submitData.detail = res.info.data[0].detail;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.latitude = res.info.data[0].latitude;
					self.submitData.longitude = res.info.data[0].longitude;
					self.submitData.isdefault = res.info.data[0].isdefault;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},



			addressUpdate() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('修改成功', 'success');
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'error')
					};

				};
				self.$apis.addressUpdate(postData, callback);
			},


			addressAdd() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功', 'success');
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'success')
					}

				};
				self.$apis.addressAdd(postData, callback);
			},


			submit() {
				const self = this;

				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.default;
				const pass = self.$Utils.checkComplete(newObject);

				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
						return;
					}
					if (self.id) {

						self.addressUpdate();
					} else {

						self.addressAdd();
					}


				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'success');
				};
			},

		}
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";

	.myRowBetween .item {
		padding: 30rpx 4%
	}
</style>
