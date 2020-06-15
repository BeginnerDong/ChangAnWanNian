<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">风云际会</view>
			</view>
			
		<view class="pageBox" :style="{top:statusBar + 'px'}">	
			<view class="d-flex j-center a-center" style="margin-top: 150rpx;">
				<view class="Dossier position-relative d-flex j-center a-center">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
						<view class="rr ml-2">
							<view class="font-40 font-weight"><open-data type="userNickName"></open-data></view>
							<view class="d-flex a-center mt-3 font-30">
								<view class="lableIcon mr-1"><image src="../../static/images/racel-icon1.png" mode=""></image></view>
								<view class="">积分：{{userInfoData.yb_score?userInfoData.yb_score:'0.00'}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view v-if="!isShare">
				<view class="">
					<view style="width: 90rpx;height: 90rpx; margin: 50rpx auto;"><image src="../../static/images/racel-icon.png" mode=""></image></view>
				</view>
				
				<view class="d-flex j-center a-center">
					<view class="Dossier position-relative d-flex j-center a-center">
						<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
						<button style="background: none;border:none" v-if="set_id!=''" class="infor d-flex a-center" open-type="share">
							<view class="photo"><image src="../../static/images/racel-img.png" mode=""></image></view>
							<view class="rr ml-2">
								<view class="font-30 font-weight">点击邀请</view>
							</view>
						</button>
						<button style="background: none;border:none" v-if="set_id==''" class="infor d-flex a-center" @click="noshare">
							<view class="photo"><image src="../../static/images/racel-img.png" mode=""></image></view>
							<view class="rr ml-2">
								<view class="font-30 font-weight">点击邀请</view>
							</view>
						</button>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pdtb15">
				<button class="btn" type="button"  open-type="getUserInfo"  @getuserinfo="submit">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">进入对战</view>
				</button>
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
				Router:this.$Router,
				userInfoData:{},
				isReady:false,
				set_id:'',
				isShare:false,
				idArray:[],
				statusBar:app.globalData.statusBar
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
				if(options.id){
					self.shareSetId = options.id
					uni.setStorageSync('setId',self.shareSetId);
				}
				
				self.$Utils.loadAll(['getSheetData'], self);
				
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
			
			
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
			
			getSheetData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type:2,
					finish:0,
					deadline:['>',(new Date()).getTime() / 1000],
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					set:{
						tableName:'Set',
						middleKey:'set_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					},
					subject:{
						tableName:'Subject',
						middleKey:['set',0,'subject_array'],
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					},
					log:{
						tableName:'Log',
						middleKey:'id',
						key:'sheet_id',
						searchItem:{
							status:1,
						},
						condition:'in',
						order:{
							create_time:'asc'
						}
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.sheetData = res.info.data[0];
						//self.getNofinishData();
						uni.showModal({
							title:'提示',
							content:'您有未完成的对战，是否立即前往继续答题？',
							showCancel:true,
							confirmText:'继续',
							success(res) {
								if(res.confirm){
									uni.setStorageSync('subjectData',self.sheetData.subject);
									uni.setStorageSync('sheetId',self.sheetData.id);
									uni.setStorageSync('setId',self.sheetData.set[0].id);
									self.$Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
								}else{
									console.log('取消')
									
								}
							}
						})
					}else{
						self.getUserInfoData()
					}
					self.$Utils.finishFunc('getSheetData');
				};
				self.$apis.sheetGet(postData, callback);
			},
			
			noshare(){
				uni.showModal({
					title:'提示',
					content:'未生成题目，不能邀请对战',
					showCancel:false,
					confirmText:'确定',
					success(res) {
						if(res.confirm){
							console.log('确定')
						}else{
							console.log('取消')
						}
					}
				})
				return
			},
			
			submit(){
				const self = this;
				const callback = (user, res) => {
					if(!uni.getStorageSync('subjectData')){
						uni.showModal({
							title:'错误',
							content:'未生成题目，无法开启对战',
							showCancel:false,
							confirmText:'确定',
							success(res) {
								if(res.confirm){
									console.log('确定')
								}else{
									console.log('取消')
								}
							}
						})
					}else{
						if(self.isShare){
							self.shareSheetAdd()
						}else{
							self.$Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
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
					id:id,
					user_type:0
				};
				postData.getAfter = {
					subject:{
						tableName:'Subject',
						middleKey:'subject_array',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					},
					sheet:{
						tableName:'Sheet',
						middleKey:'id',
						key:'set_id',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'in'
					},
				}
				var callback = function(res) {
					if (res.info.data&&res.info.data.length > 0 && res.info.data[0]) {
						self.setData = res.info.data[0]
						if(self.setData.deadline<=(new Date()).getTime() / 1000){
							uni.showModal({
								title:'结束',
								content:'本轮对战已结束',
								showCancel:false,
								confirmText:'确定',
								success(res) {
									if(res.confirm){
										self.$Router.redirectTo({route:{path:'/pages/index/index'}})
									}else{
										console.log('取消')
									}
								}
							})
							return
						}
						if(self.setData.sheet.length>=uni.getStorageSync('user_info').thirdApp.battle_limit){
							uni.showModal({
								title:'提示',
								content:'该局对战已达人数限制',
								showCancel:false,
								confirmText:'确定',
								success(res) {
									if(res.confirm){
										self.$Router.redirectTo({route:{path:'/pages/index/index'}})
									}else{
										console.log('取消')
									}
								}
							})
							return
						}
						uni.setStorageSync('setId',self.setData.id);
						uni.setStorageSync('subjectData',self.setData.subject);
					}else{
						self.$Utils.showToast(res.msg,'none');
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
					type:2,
					set_id:self.setData.id,
					deadline:self.setData.deadline,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId',res.info.id);
						self.$Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
					}
				};
				self.$apis.sheetAdd(postData, callback);
			},
			
			getSubjectData() {
				var self = this;
				uni.showLoading();
				if(self.userInfoData.member_time<(new Date()).getTime() / 1000){
					if((parseFloat(self.userInfoData.yb_score)+parseFloat(self.userInfoData.yb_today))<10){
						uni.showModal({
							title:'提示',
							content:'您的元宝积分不足，是否立即去充值',
							showCancel:true,
							confirmText:'前往',
							success(res) {
								if(res.confirm){
									self.$Router.redirectTo({route:{path:'/pages/uesr-lngotRecharge/uesr-lngotRecharge'}})
								}else{
									console.log('取消')
								}
							}
						})
					}else{
						const postData = {};
						
						postData.tokenFuncName = 'getProjectToken';
						postData.data = {
							//type:7,
							//count:uni.getStorageSync('user_info').thirdApp.yuanbao - parseFloat(self.userInfoData.yb_today),
							trade_info:'对战花费元宝',
							account:1,
							thirdapp_id:2,
							user_no:uni.getStorageSync('user_info').user_no
						};
						if(parseFloat(self.userInfoData.yb_today)>10){
							postData.data.type = 7;
							postData.data.count = -10;
						}else{
							postData.data.type = 7;
							postData.data.count = -parseFloat(self.userInfoData.yb_today);
							postData.saveAfter = [
								{
									tableName: 'FlowLog',
									FuncName: 'add',
									data: {
										type:6,
										count:-(10-parseFloat(self.userInfoData.yb_today)),
										trade_info:'对战花费元宝',
										account:1,
										thirdapp_id:2,
										user_no:uni.getStorageSync('user_info').user_no
									},
								},
							];
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
									if (res.info.data&&res.info.data.length > 0 && res.info.data[0]) {
										uni.setStorageSync('subjectData',res.info.data);
										self.subjectData = res.info.data;
										for (var i = 0; i < res.info.data.length; i++) {
											self.idArray.push(res.info.data[i].id) 
										};
										self.setAdd();
									}else{
										self.$Utils.showToast(res.msg,'none');
									}
								};
								self.$apis.subjectGet(c_postData, c_callback);
							}
						};
						self.$apis.flowLogAdd(postData, callback);
					}
				}else{
					var postData = {};
					postData.tokenFuncName = 'getProjectToken';
					postData.noLoading = true;
					postData.type = 1;
					postData.free = 0;
					postData.num = uni.getStorageSync('user_info').thirdApp.battle_num;
					var callback = function(res) {
						if (res.info.data&&res.info.data.length > 0 && res.info.data[0]) {
							uni.setStorageSync('subjectData',res.info.data);
							self.subjectData = res.info.data;
							for (var i = 0; i < res.info.data.length; i++) {
								self.idArray.push(res.info.data[i].id) 
							};
							self.setAdd();
						}else{
							self.$Utils.showToast(res.msg,'none');
						}
					};
					self.$apis.subjectGet(postData, callback);
				}
			},
			
			setAdd() {
				var self = this;
				var postData = {};
				var nowTime = (new Date()).getTime() / 1000;
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					type:2,
					subject_array:self.idArray,
					deadline:nowTime+uni.getStorageSync('user_info').thirdApp.battle_time*60,
					free:1
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000&&res.info.id) {
						self.set_id = res.info.id;
						uni.setStorageSync('setId',self.set_id);
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
					type:2,
					set_id:id,
					deadline:nowTime+uni.getStorageSync('user_info').thirdApp.battle_time*60,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId',res.info.id);
						uni.hideLoading();
						uni.showModal({
							title:'生成成功',
							content:'题目生成成功，快去邀请好友吧',
							showCancel:false,
							confirmText:'确定',
							success(res) {
								if(res.confirm){
									
								}else{
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
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						if(self.shareSetId){
							self.isShare = true;
							
							self.getSetData(self.shareSetId)
						}else{
							uni.showModal({
								title:'生成新的对战',
								content:self.userInfoData.member_time<(new Date()).getTime() / 1000?'是否确定生成对战题目，将花费您10元宝积分':'是否确定生成对战题目，会员免费',
								showCancel:true,
								confirmText:'继续',
								success(res) {
									if(res.confirm){
										self.getSubjectData()
									}else{
										console.log('取消')
									}
								}
							})
						}
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	
	button::after{
	 
	border: none;
	 
	}
</style>
