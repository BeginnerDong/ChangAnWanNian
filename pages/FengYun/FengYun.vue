<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="back()">
					<image src="../../static/images/back-icon.png" mode=""></image>
				</view>
				<view class="headBj">
				</view>
				<view class="tit">风云际会</view>
			</view>



			<view class="pageBox pb-4" :style="{marginTop:statusBar+44 + 'px'}">
				
				<view class="d-flex j-center" @click="Router.reLaunch({route:{path:'/pages/index/index'}})" style="color: #d0b487;width: 100%;
				text-align: center;text-decoration: underline;margin-top: 50rpx;font-weight: 700;">
						<image src="../../static/images/home.png" style="width: 40rpx;height: 40rpx;margin-right: 20rpx;"></image>
						进入崇德含光文化平台
					</view>

				<view class="" @click="UpperContentShow" style="color: #d0b487;width: 100%;text-align: center;text-decoration: underline;margin-top: 50rpx;">
					"风云际会"攻略
				</view>

				

				<view class="d-flex j-center a-center">
					<view class="Dossier position-relative d-flex j-center a-center">
						<view class="position-absoluteXY">
							<image src="../../static/images/the-results-ofl-icon6.png" mode=""></image>
						</view>
						<!-- <view class="infor d-flex a-center">
							<view class="photo" style="overflow: hidden;">
								<open-data type="userAvatarUrl"></open-data>
							</view>
							<view class="rr ml-2">
								<view class="font-40 font-weight">
									<open-data type="userNickName"></open-data>
								</view>
								<view class="d-flex a-center mt-3 font-30">
									<view class="lableIcon mr-1">
										<image src="../../static/images/racel-icon1.png" mode=""></image>
									</view>
									<view class="">积分：{{yb?yb:'0.00'}}</view>
								</view>
							</view>
						</view> -->
						<view class="infor">
							<view class="d-flex a-center j-center">
								<view class="photo" style="overflow: hidden;">
									<open-data type="userAvatarUrl"></open-data>
								</view>
								<view class="font-40 font-weight pl-3">
									<open-data type="userNickName"></open-data>
								</view>
							</view>
							<view class="rr ml-2">
								<view class="mt-3 font-26">
									<view class="d-flex a-center">
										<view class="lableIcon mr-1">
											<image src="../../static/images/the-results-icon7.png" mode=""></image>
										</view>
										 <view class="">
											等级：{{userInfoData.levelName&&userInfoData.levelName.length>0?userInfoData.levelName[0].title:'无'}}
										</view>
									</view>
									<view class="d-flex a-center mt-3">
										<view class="lableIcon mr-1"><image src="../../static/images/racel-icon1.png" class="jbIcon"></image></view>
										<view class="">对战积分：{{userInfoData.battle_score?userInfoData.battle_score:'0.00'}}</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</view>

				<view v-if="!isShare">
					<view class="">
						<view style="width: 90rpx;height: 90rpx; margin: 50rpx auto;">
							<image src="../../static/images/racel-icon.png" mode=""></image>
						</view>
					</view>

					<view class="d-flex j-center a-center">
						<view class="Dossier position-relative d-flex j-center a-center">
							<view class="position-absoluteXY">
								<image src="../../static/images/the-results-ofl-icon6.png" mode=""></image>
							</view>
							<button style="background: none;border:none" v-if="set_id!=''" class="infor d-flex a-center" open-type="share">
								<view class="photo">
									<image src="../../static/images/racel-img.png" mode=""></image>
								</view>
								<view class="rr ml-2">
									<view class="font-30 font-weight">点击邀请</view>
								</view>
							</button>
							<button style="background: none;border:none" v-if="set_id==''" class="infor d-flex a-center" @click="noshare">
								<view class="photo">
									<image src="../../static/images/racel-img.png" mode=""></image>
								</view>
								<view class="rr ml-2">
									<view class="font-30 font-weight">点击邀请</view>
								</view>
							</button>
						</view>
					</view>
				</view>

				<view class="submitbtn pdtb15">
					<button class="btn" type="button" open-type="getUserInfo" @getuserinfo="submit">
						<view class="btnBj">
							<image src="../../static/images/anti-icon.png" mode=""></image>
						</view>
						<view class="btnTit">进入对战</view>
					</button>
				</view>


			</view>

		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="exchangeShow1 rounded20 bg-white" v-show="is_showContent">
			<view class="closebtn" @click="UpperContentShow">×</view>
			<view class="font-30" style="line-height: 50rpx;height: 100%;overflow: auto;">
				<view class="text-center" >“风云际会”为对战模块。</view>
				
				<view>两人对战：将对战邀请发送至好友微信；</view>
				
				<view>多人对战：将对战邀请发送至好友微信群（对战总人数不多于5人）；</view>
				
				<view>单人模式：自嗨模式，不选择对手，直接点击“进入对战”。</view>
				
				<view>崇德含光鼓励用户多使用单人模式（自嗨模式）进行娱乐和学习，在本模块中每日可无限次答题（会员免费），用户可在此对知识进行巩固和温习，以期“温故而知新”。</view>
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
				userInfoData: {},
				isReady: false,
				set_id: '',
				isShare: false,
				idArray: [],
				statusBar: app.globalData.statusBar,
				yb: 0,
				is_showContent: false,
				is_show: false,
			}
		},

		onLoad(options) {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				uni.removeStorageSync('subjectData');
				uni.removeStorageSync('setId');
				uni.removeStorageSync('sheetId');
				uni.removeStorageSync('score');
				uni.removeStorageSync('sheetType');
				if (options.id) {
					self.shareSetId = options.id
					uni.setStorageSync('setId', self.shareSetId);
				}

				self.$Utils.loadAll(['getSheetData'], self);

			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})


		},

		onShow() {
			const self = this;

		},


		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '一起来答题吧！',
					path: '/pages/FengYun/FengYun?id=' + self.set_id, //点击分享的图片进到哪一个页面
					//imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			} else {
				return {
					title: '一起来答题吧！',
					path: '/pages/FengYun/FengYun?id=' + self.set_id, //点击分享的图片进到哪一个页面
					//imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
		},

		methods: {

			UpperContentShow() {
				const self = this;
				self.is_show = !self.is_show;
				self.is_showContent = !self.is_showContent
			},

			back() {
				const self = this;
				if (self.isShare) {
					self.$Router.redirectTo({
						route: {
							path: '/pages/index/index'
						}
					})
				} else {
					uni.navigateBack({
						delta: 1
					})
				}
			},

			deleteSheet() {
				var self = this;
				self.sheetData = {};
				var postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					status: -1
				};
				postData.searchItem = {
					id: self.sheetData.id
				};
				var callback = function(res) {};
				self.$apis.sheetUpdate(postData, callback);
			},

			getSheetData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type: 2,
					finish: 0,
					deadline: ['>', (new Date()).getTime() / 1000],
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					set: {
						tableName: 'Set',
						middleKey: 'set_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: 'in'
					},
					subject: {
						tableName: 'Subject',
						middleKey: ['set', 0, 'subject_array'],
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: 'in'
					},
					log: {
						tableName: 'Log',
						middleKey: 'id',
						key: 'sheet_id',
						searchItem: {
							status: 1,
						},
						condition: 'in',
						order: {
							create_time: 'asc'
						}
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.sheetData = res.info.data[0];
						//self.getNofinishData();
						uni.showModal({
							title: '提示',
							content: '您有未完成的对战，是否立即前往继续答题？',
							showCancel: true,
							confirmText: '继续',
							cancelText: '忽略',
							success(res) {
								if (res.confirm) {
									uni.setStorageSync('subjectData', self.sheetData.subject);
									uni.setStorageSync('sheetId', self.sheetData.id);
									uni.setStorageSync('setId', self.sheetData.set[0].id);
									uni.setStorageSync('sheetType', 2);
									self.$Router.navigateTo({
										route: {
											path: '/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'
										}
									})
								} else {
									self.deleteSheet();
									self.getUserInfoData()
								}
							}
						})
					} else {
						self.getUserInfoData()
					}
					self.$Utils.finishFunc('getSheetData');
				};
				self.$apis.sheetGet(postData, callback);
			},

			noshare() {
				uni.showModal({
					title: '提示',
					content: '未生成题目，不能邀请对战',
					showCancel: false,
					confirmText: '确定',
					success(res) {
						if (res.confirm) {
							console.log('确定')
						} else {
							console.log('取消')
						}
					}
				})
				return
			},

			submit() {
				const self = this;
				const callback = (user, res) => {
					if (!uni.getStorageSync('subjectData')) {
						uni.showModal({
							title: '错误',
							content: '未生成题目，无法开启对战',
							showCancel: false,
							confirmText: '确定',
							success(res) {
								if (res.confirm) {
									console.log('确定')
								} else {
									console.log('取消')
								}
							}
						})
					} else {
						if (self.isShare) {
							self.shareSheetAdd()
						} else {
							uni.setStorageSync('sheetType', 2);
							self.$Router.navigateTo({
								route: {
									path: '/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'
								}
							})
						}
					}
				};
				self.$Utils.getAuthSetting(callback);
			},

			getSetData(id) {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: id,
					user_type: 0
				};
				postData.getAfter = {
					subject: {
						tableName: 'Subject',
						middleKey: 'subject_array',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: 'in'
					},
					sheet: {
						tableName: 'Sheet',
						middleKey: 'id',
						key: 'set_id',
						searchItem: {
							status: 1,
							user_type: 0
						},
						condition: 'in'
					},
				}
				var callback = function(res) {
					if (res.info.data && res.info.data.length > 0 && res.info.data[0]) {
						self.setData = res.info.data[0]
						if (self.setData.deadline <= (new Date()).getTime() / 1000) {
							uni.showModal({
								title: '结束',
								content: '本轮对战已结束',
								showCancel: false,
								confirmText: '确定',
								success(res) {
									if (res.confirm) {
										self.$Router.redirectTo({
											route: {
												path: '/pages/index/index'
											}
										})
									} else {
										console.log('取消')
									}
								}
							})
							return
						}
						if (self.setData.sheet.length >= uni.getStorageSync('user_info').thirdApp.battle_limit) {
							uni.showModal({
								title: '提示',
								content: '该局对战已达人数限制',
								showCancel: false,
								confirmText: '确定',
								success(res) {
									if (res.confirm) {
										self.$Router.redirectTo({
											route: {
												path: '/pages/index/index'
											}
										})
									} else {
										console.log('取消')
									}
								}
							})
							return
						}
						for (var i = 0; i < self.setData.sheet.length; i++) {
							if (self.setData.sheet[i].user_no == uni.getStorageSync('user_info').user_no) {
								var meSheet = self.setData.sheet[i]
							}
						};
						if (meSheet && meSheet.finish == 1) {
							uni.showModal({
								title: '提示',
								content: '您已完成该局对战，是否查看结果',

								confirmText: '查看',
								cancelText: '返回首页',
								success(res) {
									if (res.confirm) {
										self.$Router.redirectTo({
											route: {
												path: '/pages/FengYun-ResultRanking/FengYun-ResultRanking'
											}
										})
									} else {
										self.$Router.redirectTo({
											route: {
												path: '/pages/index/index'
											}
										})
									}
								}
							})
							return
						}
						uni.setStorageSync('setId', self.setData.id);
						uni.setStorageSync('subjectData', self.setData.subject);
					} else {
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.setGet(postData, callback);
			},

			shareSheetAdd(id) {
				var self = this;

				var postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.refreshToken = true;
				postData.data = {
					type: 2,
					set_id: self.setData.id,
					deadline: self.setData.deadline,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId', res.info.id);
						uni.setStorageSync('sheetType', 2);
						self.$Router.navigateTo({
							route: {
								path: '/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'
							}
						})
					}
				};
				self.$apis.sheetAdd(postData, callback);
			},

			getSubjectData() {
				var self = this;
				uni.showLoading();
				if (self.userInfoData.member_time < (new Date()).getTime() / 1000) {
					if ((parseFloat(self.userInfoData.yb_score) + parseFloat(self.userInfoData.yb_today)) < 10) {
						uni.showModal({
							title: '提示',
							content: '您的元宝积分不足，是否立即去充值',
							showCancel: true,
							confirmText: '前往',
							success(res) {
								uni.hideLoading();
								if (res.confirm) {
									self.$Router.redirectTo({
										route: {
											path: '/pages/uesr-lngotRecharge/uesr-lngotRecharge'
										}
									})
								} else {
									console.log('取消')
								}
							}
						})
					} else {
						const postData = {};

						postData.tokenFuncName = 'getProjectToken';
						postData.data = {
							//type:7,
							//count:uni.getStorageSync('user_info').thirdApp.yuanbao - parseFloat(self.userInfoData.yb_today),
							trade_info: '对战花费元宝',
							account: 1,
							thirdapp_id: 2,
							user_no: uni.getStorageSync('user_info').user_no
						};
						if (parseFloat(self.userInfoData.yb_today) > 10) {
							postData.data.type = 7;
							postData.data.count = -10;
						} else {
							postData.data.type = 7;
							postData.data.count = -parseFloat(self.userInfoData.yb_today);
							postData.saveAfter = [{
								tableName: 'FlowLog',
								FuncName: 'add',
								data: {
									type: 6,
									count: -(10 - parseFloat(self.userInfoData.yb_today)),
									trade_info: '对战花费元宝',
									account: 1,
									thirdapp_id: 2,
									user_no: uni.getStorageSync('user_info').user_no
								},
							}, ];
						};
						const callback = (res) => {
							if (res.solely_code == 100000) {
								var c_postData = {};
								c_postData.tokenFuncName = 'getProjectToken';
								c_postData.noLoading = true;
								c_postData.type = 1;
								c_postData.free = 0;
								c_postData.num = uni.getStorageSync('user_info').thirdApp.battle_num;
								var c_callback = function(res) {
									if (res.info.data && res.info.data.length > 0 && res.info.data[0]) {
										uni.setStorageSync('subjectData', res.info.data);
										self.subjectData = res.info.data;
										for (var i = 0; i < res.info.data.length; i++) {
											self.idArray.push(res.info.data[i].id)
										};
										self.setAdd();
									} else {
										self.$Utils.showToast(res.msg, 'none');
									}
								};
								self.$apis.getSubject(c_postData, c_callback);
							}
						};
						self.$apis.flowLogAdd(postData, callback);
					}
				} else {
					var postData = {};
					postData.tokenFuncName = 'getProjectToken';
					postData.noLoading = true;
					postData.type = 1;
					postData.free = 0;
					postData.num = uni.getStorageSync('user_info').thirdApp.battle_num;
					var callback = function(res) {
						if (res.info.data && res.info.data.length > 0 && res.info.data[0]) {
							uni.setStorageSync('subjectData', res.info.data);
							self.subjectData = res.info.data;
							for (var i = 0; i < res.info.data.length; i++) {
								self.idArray.push(res.info.data[i].id)
							};
							self.setAdd();
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					};
					self.$apis.getSubject(postData, callback);
				}
			},

			setAdd() {
				var self = this;
				var postData = {};
				var nowTime = (new Date()).getTime() / 1000;
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					type: 2,
					subject_array: self.idArray,
					deadline: nowTime + uni.getStorageSync('user_info').thirdApp.battle_time * 60,
					free: 1
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000 && res.info.id) {
						self.set_id = res.info.id;
						uni.setStorageSync('setId', self.set_id);
						self.sheetAdd(res.info.id)
					}
				};
				self.$apis.setAdd(postData, callback);
			},



			sheetAdd(id) {
				var self = this;
				var nowTime = (new Date()).getTime() / 1000;
				var postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					type: 2,
					set_id: id,
					deadline: nowTime + uni.getStorageSync('user_info').thirdApp.battle_time * 60,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId', res.info.id);
						uni.hideLoading();
						uni.showModal({
							title: '生成成功',
							content: '题目生成成功，快去邀请好友吧',
							showCancel: false,
							confirmText: '确定',
							success(res) {
								if (res.confirm) {

								} else {
									console.log('取消')
								}
							}
						})
					}
				};
				self.$apis.sheetAdd(postData, callback);
			},

			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					levelName: {
						tableName: 'Level',
						middleKey: 'level',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '='
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.yb = parseFloat(parseFloat(self.userInfoData.yb_score) + parseFloat(self.userInfoData.yb_today)).toFixed(2)
						if (self.shareSetId) {
							self.isShare = true;

							self.getSetData(self.shareSetId)
						} else {
							uni.showModal({
								title: '生成新的对战',
								content: self.userInfoData.member_time < (new Date()).getTime() / 1000 ? '是否确定生成对战题目，将花费您10元宝积分' : '是否确定生成对战题目，会员免费',
								showCancel: true,
								confirmText: '继续',
								success(res) {
									if (res.confirm) {
										self.getSubjectData()
									} else {
										console.log('取消')
									}
								}
							})
						}
					};
					//self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";

	button::after {

		border: none;

	}
	.exchangeShow1 {
		width: 80%;
		height: 550rpx;
		padding: 70rpx 50rpx 20rpx 50rpx;
		position: fixed;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
		z-index: 50;
	}
</style>
