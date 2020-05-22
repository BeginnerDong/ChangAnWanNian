<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">日积月累</view>
			</view>
			
			
			<view class="d-flex j-center a-center" style="margin-top: 220rpx;">
				
				<view class="Dossier position-relative d-flex j-center a-center">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
						<view class="rr ml-2">
							<view class="font-40 font-weight"><open-data type="userNickName"></open-data></view>
							<view class="d-flex a-center mt-3 font-26">
								<view class="lableIcon mr-1"><image src="../../static/images/the-results-icon7.png" mode=""></image></view>
								<view class="">等级：{{userInfoData.levelName&&userInfoData.levelName.length>0?userInfoData.levelName[0].title:'无'}}</view>
							</view>
						</view>
					</view>
				</view>
				
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top:180rpx;">
				<button class="btn" type="button" open-type="getUserInfo"  @getuserinfo="submit">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">开始答题</view>
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
				userInfoData:{},
				canFirstFree:true,
				canTodayFree:true,
				idArray:[]
			}
		},
		
		onLoad() {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				uni.removeStorageSync('subjectData');
				uni.removeStorageSync('setId');
				uni.removeStorageSync('sheetId');
				uni.removeStorageSync('score');
				self.$Utils.loadAll(['getUserInfoData','getFirstFreeData','getTodayFreeData','getSheetData'], self);
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
		},
		
		methods: {
			
			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					levelName:{
						tableName:'Level',
						middleKey:'level',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getFirstFreeData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					free:2,
					type:1,
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.canFirstFree = false
					};
					self.$Utils.finishFunc('getFirstFreeData');
				};
				self.$apis.setGet(postData, callback);
			},
			
			getTodayFreeData() {
				var self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = (new Date()).getTime() / 1000;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					free:1,
					type:1,
					create_time:['between',[dayStart,nowTime]],
					user_no:uni.getStorageSync('user_info').user_no
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.canTodayFree = false
					};
					self.$Utils.finishFunc('getTodayFreeData');
				};
				self.$apis.setGet(postData, callback);
			},
			
			getSheetData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					type:1,
					finish:0,
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
						
					};
					self.$Utils.finishFunc('getSheetData');
				};
				self.$apis.sheetGet(postData, callback);
			},
			
			
			
			submit(){
				const self = this;
				const callback = (user, res) => {
					if(self.sheetData){
						uni.showModal({
							title:'提示',
							content:'您有未完成的付费题目，是否立即前往继续答题？',
							showCancel:true,
							confirmText:'继续',
							success(res) {
								if(res.confirm){
									uni.setStorageSync('subjectData',self.sheetData.subject.splice(0,self.sheetData.log.length));
									uni.setStorageSync('sheetId',self.sheetData.id);
									self.$Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
								}else{
									console.log('取消')
								}
							}
						})
					}else{
						if(!self.canFirstFree&&!self.canTodayFree){
							self.Router.navigateTo({route:{path:'/pages/buildUp-Result-Pay/buildUp-Result-Pay?type=2'}})
							return
						}else{
							self.getSubjectData()
						}
					}
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			getSubjectData() {
				var self = this;
				uni.showLoading();
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.type = 0;
				if(self.canFirstFree){
					postData.free = 1;
					postData.num = 10
				}else if(self.canTodayFree){
					postData.free = 0;
					postData.num = 5
				}
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
			},
			
			setAdd() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					type:1,
					subject_array:self.idArray,
				};
				if(self.canFirstFree){
					postData.data.free = 2
				}else if(self.canTodayFree){
					postData.data.free = 1
				}else{
					postData.data.free = 0
				}
				var callback = function(res) {
					if (res && res.solely_code == 100000&&res.info.id) {
						self.sheetAdd(res.info.id)
					}
				};
				self.$apis.setAdd(postData, callback);
			},
			
			sheetAdd(id) {
				var self = this;
				var postData = {};
				postData.noLoading = true;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					type:1,
					set_id:id,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId',res.info.id);
						uni.hideLoading();
						self.$Router.navigateTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
					}
				};
				self.$apis.sheetAdd(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
</style>
