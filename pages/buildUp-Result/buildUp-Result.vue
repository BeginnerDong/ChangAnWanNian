<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">结果</view>
			</view>
			
			<view class="pageBox">
			<view class="d-flex j-center a-center" style="margin-top: 150rpx;">
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
			
			<view class="text-center font-34 font-weight mt-4">恭喜你，<open-data type="userNickName"></open-data>，完成答题！</view>
			<view class="text-center font-26 py-5">
				<span class="red font-weight" style="font-size:74rpx;">{{score}}</span>分
			</view>
			
			<view class="mx-5 px-3 d-flex j-sb a-center font-28 main-text-color  text-center pt-2">
				<view class="TwoBtn position-relative" @click="toPay(1)">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-icon10.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">查看历史解析</view>
				</view>
				<view class="TwoBtn position-relative" @click="toPay(2)">
					<view class="position-absoluteXY"><image src="../../static/images/the-results-icon10.png" mode=""></image></view>
					<view class="position-relative" style="z-index: 2;">继续作答</view>
				</view>
			</view>
			
			<view class="submitbtn mt-4">
				<button class="btn" type="button" @click="toPay(3)">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">查看本轮解析</view>
				</button>
			</view>
			
			
			<!-- 达到上限弹框 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="exchangeShow rounded20 bg-white" v-show="is_UpperLimit">
				<view class="closebtn" @click="UpperLimitShow">×</view>
				<view class="text-center px-3 font-30" style="line-height: 50rpx;height: 280rpx;">今日您的回答次数已达到上限，所不能再继续作答，您可以看看其他的模板</view>
				
				
				<view class="submitbtn mt-3" >
					<button class="btn" style="width: 100%;" type="button" @click="Router.reLaunch({route:{path:'/pages/index/index'}})">
						<view class="btnBj"><image src="../../static/images/electricityl-icon1.png" mode=""></image></view>
						<view class="btnTit">去看看</view>
					</button>
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj';
	export default {
		components: {
			pageBj
		},
		data() {
			return {
				Router:this.$Router,
				is_UpperLimit:false,
				is_show:false,
				score:0,
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.score = uni.getStorageSync('score');
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		methods: {
			
			toPay(type){
				const self = this;
				if(type==2){
					if(self.userInfoData.log.length>=uni.getStorageSync('user_info').thirdApp.answer_limit){
						self.is_UpperLimit = true;
						self.is_show = true
						return
					};
					
				};
				self.Router.navigateTo({route:{path:'/pages/buildUp-Result-Pay/buildUp-Result-Pay?type='+type}})
			},
			
			getUserInfoData() {
				var self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = (new Date()).getTime() / 1000;
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
					},
					log:{
						tableName:'Log',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							type:1,
							behavior:1,
							create_time:['between',[dayStart,nowTime]]
						},
						condition:'=',
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
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:uni.getStorageSync('sheetId')
				};
				postData.getAfter = {
					set:{
						tableName:'Set',
						middleKey:'set_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.sheetUpdate()
					};
					
				};
				self.$apis.sheetGet(postData, callback);
			},
			
			sheetUpdate() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					finish:1,
					score:self.score
				};
				postData.searchItem = {
					id:self.mainData.id
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						self.$Utils.finishFunc('getMainData');
					}
				};
				self.$apis.sheetUpdate(postData, callback);
			},
			
			UpperLimitShow(){
				const self = this;
				self.is_UpperLimit = !self.is_UpperLimit;
				self.is_show = !self.is_show
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	.TwoBtn{width: 260rpx;height: 80rpx;line-height: 80rpx;}
	
	/* 弹框 */
	.exchangeShow{width:80%;height:550rpx;padding: 100rpx 50rpx 50rpx 50rpx;position: fixed;left: 50%;top: 50%;transform: translate(-50%,-50%);z-index: 50; }
	.alertBox{position: absolute;top: 50%;left: 50%;transform: translate(-50%,-50%);width: 400rpx;height: 200rpx; z-index: 55;background-color: rgba(0,0,0,0.5);}
</style>
