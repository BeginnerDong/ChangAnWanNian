<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">结果</view>
			</view>
			
		<view class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="d-flex j-center a-center" style="margin-top: 150rpx;">
				<view class="Dossier position-relative d-flex j-center a-center">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-ofl-icon6.png" mode=""></image></view>
					<view class="infor d-flex a-center">
						<view class="photo" style="overflow: hidden;"><open-data type="userAvatarUrl"></open-data></view>
						<view class="rr ml-2">
							<view class="font-40 font-weight"><open-data type="userNickName"></open-data></view>
							<view class="d-flex a-center mt-3 font-26">
								<view class="lableIcon mr-1"><image src="../../static/images/racel-icon1.png" mode=""></image></view>
								<view class="">积分：{{userInfoData.battle_score?userInfoData.battle_score:''}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="text-center font-34 font-weight mt-4">恭喜你，<open-data type="userNickName"></open-data>，完成对战！</view>
			<view class="text-center font-26 py-5">
				<span class="red font-weight" style="font-size:74rpx;">{{score}}</span>分
			</view>
			
			<view class="submitbtn mt-4">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/FengYun-ResultRanking/FengYun-ResultRanking'}})">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">查看本轮排名</view>
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
				score:0,
				userInfoData:{},
				statusBar:app.globalData.statusBar
			}
		},
		
		onLoad() {
			const self = this;
			self.score = uni.getStorageSync('score');
			self.time = uni.getStorageSync('time');
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.getAfter = {
					levelName:{
						tableName:'Level',
						middleKey:'level',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.sheetUpdate()
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			sheetUpdate() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					finish:1,
					score:self.score,
					time:self.time
				};
				postData.searchItem = {
					id:uni.getStorageSync('sheetId')
				};
				postData.saveAfter = [{
					  tableName:'FlowLog',
					  FuncName:'add',
					  data:{
						 type:5,
						 count:self.score,
						 trade_info:'对战积分',
						 account:1,
						 thirdapp_id:2,
						 user_no:uni.getStorageSync('user_info').user_no
					  }
				}];
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						self.$Utils.finishFunc('getUserInfoData');
					}
				};
				self.$apis.sheetUpdate(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	
</style>
