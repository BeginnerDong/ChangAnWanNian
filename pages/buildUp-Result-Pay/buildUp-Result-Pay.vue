<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">购买</view>
			</view>
			
		<view class="pageBox pb-4" :style="{marginTop:statusBar+44 + 'px'}">	
			<view class="px-5 mx-3 text-center" style="margin-top: 180rpx;">
				<view class="" style="height: 100rpx;" v-if="type==1">
					<view>查看历史答题解析需要购买</view>
					<view class="red mt-2">“{{logData&&logData.length}}道题{{price}}元”</view>
				</view>
				<view class="" style="height: 100rpx;" v-if="type==2&&!canTodayFree&&!isMember">
					<view>您的免费次数已经用完，需要购买</view>
					<view class="red mt-2">“{{num}}道题{{price}}元”</view>
				</view>
				<view class="" style="height: 100rpx;" v-if="type==2&&canTodayFree">
					<view>您当日还有一次免费次数</view>
					<view class="red mt-2">“5道题{{price}}元”</view>
				</view>
				<view class="" style="height: 100rpx;" v-if="type==2&&isMember">
					<view>会员用户可免费答题</view>
					<view class="red mt-2">“{{num}}道题{{price}}元”</view>
				</view>
				<view class="" style="height: 100rpx;" v-if="type==3&&isFree">
					<view>本轮增送查看解析机会</view>
				</view>
				<view class="" style="height: 100rpx;" v-if="type==3&&isMember">
					<view>会员免费查看本轮解析</view>
				</view>
				<view class="font-44 font-weight py-5 my-2">崇德含光文化平台</view>
				<view class="w-100"  style="font-size: 70rpx; border-bottom: 2px solid #cacaca;">￥{{price}}</view>
				<view class="w-100 d-flex a-center j-sb py-3" v-if="price>0">
					<view class="color6">微信支付</view>
					<view class="seltIcon" @click="change(1)"><image :src="payType==1?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
				</view>
				<view class="w-100 d-flex a-center j-sb py-3" v-if="price>0">
					<view class="color6">余额支付</view>
					<view class="seltIcon" @click="change(2)"><image :src="payType==2?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 100rpx;">
				<button class="btn" type="button" @click="submit">
					<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
					<view class="btnTit">{{price>0?'支付':'下一步'}}</view>
				</button>
			</view>
		</view>
		
		
		<!-- 弹出框 -->
		<view class="bg-mask" v-if="showToast">
			<view class="bg-white rounded20 text-center d-flex flex-column maskBox">
				<view class="font-30 py-3 font-weight">提示</view>
				<view class="flex-1 d-flex  py-2 txt" style="padding-left: 20rpx;padding-right: 20rpx;">您可以有更好的选择，余额充值（8折优惠）或购买会员月卡（畅享平台功能），请选择。</view>
				<view class="d-flex a-center bto">
					<view @click="addOrder">继续支付</view>
					<view @click="Router.navigateTo({route:{path:'/pages/uesr-Recharge/uesr-Recharge'}})">充值</view>
					<view @click="Router.navigateTo({route:{path:'/pages/user-Vip/user-Vip'}})">购买会员</view>
				</view>
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
				Router:this.$Router,
				logData:[],
				type:'',
				price:0,
				//题目数量
				num:0,
				canTodayFree:false,
				isMember:false,
				isFree:false,
				payType:1,
				idArray:[],
				statusBar:app.globalData.statusBar,
				showToast:false
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log('options',options)
			if(options.type){
				self.type = options.type
			};
			if(self.type==1){
				self.$Utils.loadAll(['getLogData'], self);
			};
			if(self.type==2){
				self.$Utils.loadAll(['getTodayFreeData'], self);
			};
			if(self.type==3){
				self.$Utils.loadAll(['getSheetData'], self);
			}
		},
		
		onShow() {
			const self = this;
			self.getUserInfoData()
		},
		
		methods: {
			
			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getSubjectData1() {
				var self = this;
				uni.showLoading();
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.type = 0;
				postData.free = 1;
				if(parseFloat(self.price)>0){

					postData.num = self.num
				}else{
					
					if(self.isMember){
						postData.num = self.num
					}else{
						postData.num = 5
					}	
				};
				var callback = function(res) {
					if(res.solely_code==100000){
						if (res.info.data&&res.info.data.length > 0 && res.info.data[0]) {
							//uni.setStorageSync('subjectData',res.info.data);
							self.subjectData = res.info.data;
							/* for (var i = 0; i < res.info.data.length; i++) {
								self.idArray.push(res.info.data[i].id) 
							};
							self.setAdd(); */
						}
					}
				};
				self.$apis.getSubject(postData, callback);
			},
			
			change(type){
				const self = this;
				console.log(type)
				if(type!=self.payType){
					self.payType = type
				}
			},
			
			submit(){
				const self = this;
				var nowTime = (new Date()).getTime() / 1000;
				if(self.type==1){
					self.addOrder()
				}else if(self.type==2){
					if(self.isMember||self.canTodayFree){
						self.getSubjectData()
					}else{
						if(!self.isMember&&self.payType==1){
							self.showToast = true
						}else{
							self.addOrder()
						}
					}
				}else if(self.type==3){
					if(self.isFree||self.isMember){
						self.$Router.navigateTo({route:{path:'/pages/buildUp-analysis/buildUp-analysis?type=3'}})
					}else{
						if(!self.isMember&&self.payType==1){
							self.showToast = true
						}else{
							self.addOrder()
						}
					}
				}
			},
			
			getSubjectData() {
				var self = this;
				uni.showLoading({
					title:'获取题目中...'
				})
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.type = 0;
				postData.free = 0;
				postData.num = self.num;
				var callback = function(res) {
					if(res.solely_code==100000){
						if (res.info.data&&res.info.data.length > 0 && res.info.data[0]) {
							uni.setStorageSync('subjectData',res.info.data);
							self.subjectData = res.info.data;
							for (var i = 0; i < res.info.data.length; i++) {
								self.idArray.push(res.info.data[i].id) 
							};
							self.setAdd();
						}else{
							self.$Utils.showToast('恭喜您已通关现有题目，题库会不定期更新，推荐您前往"风云际会"，使用单人模式（直接选择"进入对战"，不要选择对手）继续您的答题','none');
						}
					}else{
						self.$Utils.showToast(res.msg,'none');
					}
				};
				self.$apis.getSubject(postData, callback);
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
				if(self.canTodayFree){
					postData.data.free = 1
				}else{
					postData.data.free = 0
				};
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
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					type:1,
					set_id:id,
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000) {
						uni.setStorageSync('sheetId',res.info.id);
						uni.hideLoading();
						uni.setStorageSync('sheetType', 1);
						self.$Router.redirectTo({route:{path:'/pages/buildUp-Answer-Singular/buildUp-Answer-Singular'}})
					}
				};
				self.$apis.sheetAdd(postData, callback);
			},
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				const postData = {};	
				postData.tokenFuncName = 'getProjectToken',
				postData.data = {
					price:parseFloat(self.price).toFixed(2),
					level:1,
				};
				if(self.type==1){
					postData.type=5
				}else{
					postData.type=4
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					}else{
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
			
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				self.price =  parseFloat(self.price);
				const postData = {};
				if(self.payType==1){
					postData.wxPay = {
						price:parseFloat(self.price).toFixed(2),
					};
				}else if(self.payType==2){
					postData.balance = {
						price:parseFloat(self.price).toFixed(2),
					};
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [];
				if(self.payType==1){
					if(postData.wxPay&&postData.wxPay.price>0){
						postData.payAfter.push(
							{
								tableName: 'FlowLog',
								FuncName: 'add',
								data: {
									type:3,
									thirdapp_id:2,
									user_no:uni.getStorageSync('user_info').user_no,
									account:1,
									count:parseFloat(postData.wxPay.price),
									trade_info:'消费返积分'
								},
							},
						) 
					};
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										self.payAfter()
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								self.payAfter()
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			payAfter(){
				const self = this;
				if(self.type==1){
					self.$Router.navigateTo({route:{path:'/pages/buildUp-analysis/buildUp-analysis?type=1&orderId='+self.orderId}})
				}else if(self.type==2){
					if(self.subjectData){
						self.getSubjectData()
					}else{
						self.$Utils.showToast('恭喜您已通关现有题目，题库会不定期更新，推荐您前往"风云际会"，使用单人模式（直接选择"进入对战"，不要选择对手）继续您的答题', 'none');
					};
				}else if(self.type==3){
					self.$Router.navigateTo({route:{path:'/pages/buildUp-analysis/buildUp-analysis?type=3'}})
				}
			},
			
			
			getSheetData() {
				var self = this;
				var postData = {};
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = (new Date()).getTime() / 1000;
				//如果是会员，打断
				if(uni.getStorageSync('user_info').info.member_time>nowTime){
					self.$Utils.finishFunc('getSheetData');
					self.num = uni.getStorageSync('user_info').thirdApp.answer_num;
					self.isMember = true;
					return
				};
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
						self.sheetData = res.info.data[0];
						if(self.sheetData.set[0].free==2){
							self.isFree = true
						}else{
							self.price = uni.getStorageSync('user_info').thirdApp.set_price
						}
					};
					self.$Utils.finishFunc('getSheetData');
				};
				self.$apis.sheetGet(postData, callback);
			},
			
			//判断今天还有没有5次免费
			getTodayFreeData() {
				var self = this;
				var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				var nowTime = (new Date()).getTime() / 1000;
				//如果是会员，打断
				if(uni.getStorageSync('user_info').info.member_time>nowTime){
					self.num = uni.getStorageSync('user_info').thirdApp.answer_num;
					self.isMember = true;
					self.$Utils.finishFunc('getTodayFreeData');
					return
				};
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					free:1,
					type:1,
					create_time:['between',[dayStart,nowTime]]
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.num = uni.getStorageSync('user_info').thirdApp.answer_num;
						self.price =  uni.getStorageSync('user_info').thirdApp.answer_price;
						self.getSubjectData1()
					}else{
						self.canTodayFree = true
					}
					self.$Utils.finishFunc('getTodayFreeData');
				};
				self.$apis.setGet(postData, callback);
			},
			
			getLogData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage:1,
					pagesize:50,
					is_page:true,
				};
				postData.searchItem = {
					type:1,
					relation_table:'Subject',
					behavior:1,
				};
				postData.getAfter = {
					subject:{
						tableName:'Subject',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.logData.push.apply(self.logData,res.info.data)
					}
					self.price = (self.logData.length*parseFloat(uni.getStorageSync('user_info').thirdApp.unit_price)).toFixed(2)
					self.$Utils.finishFunc('getLogData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			
		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	.TwoBtn{width: 260rpx;height: 80rpx;line-height: 80rpx;}
	.bg-mask{position: fixed;top: 0;left: 0;right: 0;bottom: 0;background-color: rgba(0,0,0,0.5);z-index: 1000;}
	.maskBox{width: 600rpx;position: absolute;top: 30%;left: 0;right: 0;margin: auto;}
	.maskBox .txt{min-height: 200rpx;border-top: 1px solid #e1e1e1;border-bottom: 1px solid #e1e1e1;}
	.maskBox .bto view{width:33.33%;line-height: 80rpx;border-right: 1px solid #e1e1e1;}
	.maskBox .bto view:nth-child(3){border-right: none;}
</style>
