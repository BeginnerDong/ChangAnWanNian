<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">答题</view>
			</view>
			
			<view class="pageBox pb-4" :style="{marginTop:statusBar+ 44 + 'px'}">	
				<view class="d-flex j-center a-center" style="margin-top: 80rpx;">
					<view class="answerTime position-relative text-center">
						<view class="position-absoluteXY"><image src="../../static/images/anti-img.png" mode=""></image></view>
						<view class="infor pt-2">
							<view class="font-24 color6">
								<span class="color2 font-weight mr-1" style="font-size: 50rpx;">{{Utils.numToChinese(currentIndex+1)}}</span>
								<span v-if="currentSubject.type==1||currentSubject.type==5">单选</span>
								<span v-if="currentSubject.type==2" style="color: red;">多选</span>
								<span v-if="currentSubject.type==3">填空</span>
								<span v-if="currentSubject.type==4">选词</span>
							</view>
							<view class="font-26 mt" v-if="time>0">倒计时：
							<span class="red font-28">{{time}}秒</span>
							</view>
						</view>
					</view>
				</view>
				
				<view class="answerTit px-4 py-5 font-34 mt-2">
					<view>
						<view class="content ql-editor" style="padding:0;" v-html="currentSubject.title">
						</view>
					</view>
					<view style="width: 100%;height: 200px;" v-for="(item,index) in currentSubject.mainImg" :key="index" @click="previewImage(index)">
						<image style="width: 100%;height: 100%;" :src="item.url"></image>
					</view>
					<view class="d-flex j-end" v-if="currentSubject.battle==0">
						<view class="zanBtn mr-1" @click="Utils.stopMultiClick(clickGood)">
							<image  :src="logData&&logData.status&&logData.status==1?'../../static/images/explorel-icon2.png':'../../static/images/explorel-icon1.png'" mode=""></image>
						</view>
					</view>
					<view class="pt-2 pb-1 font-30" v-if="currentSubject.type&&currentSubject.type==4" style="border-bottom: 1px solid #999; vertical-align: middle;">
						<view class="d-inline-block" style="position: relative;width: 100%;">{{text}}
							<image v-if="hasResult&&!right" class="d-inline-block errorIcon mt ml-2" 
							style="vertical-align: baseline;" src="../../static/images/anti-icon4.png" mode="">
							</image>
							<image v-if="hasResult&&right" class="d-inline-block rightIcon mt ml-2" style="vertical-align: baseline;" 
							src="../../static/images/anti-icon3.png" mode=""></image>
							<image v-if="!hasResult&&text.length>0" @click="deleteText" class="d-inline-block" style="width: 50rpx;height:50rpx;position: absolute;right: 0;"
							src="../../static/images/anti-icon10.png" mode=""></image>
						</view>
					</view>
				</view>
				
				<view  v-if="currentSubject.type&&currentSubject.type==1">
					<view class="submitbtn pt-5">
						<button class="btn mb-3"  v-for="(item,index) in currentSubject.Option" :key="index" :data-index="index" 
						@click="!hasResult?answer($event.currentTarget.dataset.index):''"
						type="button">
							<view class="seltIconL errorIcon" v-if="chooseIndex==index&&chooseIndex!=rightIndex">
								<image src="../../static/images/anti-icon4.png" mode=""></image></view>
							<view class="btnBj" v-if="chooseIndex==index&&chooseIndex!=rightIndex"><image src="../../static/images/anti-icon1.png" mode=""></image></view>
							<view class="btnBj" v-if="chooseIndex!=index&&index!=rightIndex"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnBj" v-if="index==rightIndex"><image src="../../static/images/anti-icon2.png" mode=""></image></view>
							<view class="btnTit color2" style="z-index: 999;">{{item.option}}</view>
							<view class="seltIconR rightIcon" v-if="index==rightIndex"><image src="../../static/images/anti-icon3.png" mode=""></image></view>
						</button>
					</view>
					<!-- <view class="submitbtn mt-5 pt-3" v-if="!hasResult">
						<button class="btn" type="button" @click="answerConfirm">
							<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnTit" style="z-index: 999;">确定</view>
						</button>
					</view> -->
				</view>
				
				<view  v-if="currentSubject.type&&currentSubject.type==2">
					<view class="submitbtn pt-5">
						<button class="btn mb-3" :data-index="index" 
					@click="!hasResult?dxChoose($event.currentTarget.dataset.index):''" v-for="(item,index) in currentSubject.Option" :key="index" type="button">
							<view class="seltIconL errorIcon" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)<0"><image src="../../static/images/anti-icon4.png" mode=""></image></view>
							<view class="btnBj" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)<0"><image src="../../static/images/anti-icon1.png" mode=""></image></view>
							<view class="btnBj" v-if="!hasResult||(hasResult&&Utils.inArray(index,chooseArray)<0&&Utils.inArray(index,rightArray)<0)"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnBj" v-if="hasResult&&Utils.inArray(index,rightArray)>=0"><image src="../../static/images/anti-icon2.png" mode=""></image></view>
							<view class="btnTit color2" style="z-index: 999;" :class="Utils.inArray(index,chooseArray)>=0?'colorRed':''">{{item.option}}</view>
							<view class="seltIconR rightIcon" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)>=0"><image src="../../static/images/anti-icon3.png" mode=""></image></view>
						</button>
					</view>
					<view class="submitbtn mt-5 pt-3" v-if="!hasResult">
						<button class="btn" type="button" @click="dxConfirm">
							<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnTit" style="z-index: 999;">确定</view>
						</button>
					</view>
				</view>
				
				
				<view v-if="currentSubject.type&&currentSubject.type==3">
					<view class="mx-4 d-flex a-center j-center pt-5">
						<view class="fillInBox position-relative" v-if="!hasResult">
							<view class="position-absoluteXY"><image src="../../static/images/anti-icon5.png" mode=""></image></view>
							<view class="infor">
								<textarea v-model="text" placeholder="填写答案(相连的空连续填写，不相连的空之间用'空格键'分开)" placeholder-class="placeholder" />
							</view>
						</view>
						<view class="fillInBox position-relative" style="height: 300rpx;padding-bottom: 88rpx;" v-if="hasResult">
							<view class="position-absoluteXY" v-if="!right"><image src="../../static/images/anti-icon6.png" mode=""></image></view>
							<view class="position-absoluteXY" v-if="right"><image src="../../static/images/anti-icon7.png" mode=""></image></view>
							<view class="infor">
								<scroll-view scroll-y="true" style="height: 100%;">
									<view>{{text}}</view>
								</scroll-view>
							</view>
							<view class="seltIconL errorIcon" v-if="!right" style="top: auto;bottom: 34rpx;left: 50%;transform: translate(-50%,0);">
								<image src="../../static/images/anti-icon4.png" mode=""></image>
							</view>
							<view class="seltIconL rightIcon" v-if="right" style="top: auto;bottom: 34rpx;left: 50%;transform: translate(-50%,0);">
								<image src="../../static/images/anti-icon3.png" mode=""></image>
							</view>
						</view>
					</view>
					<view class="submitbtn mt-5" v-if="!hasResult">
						<button class="btn mb-3"  type="button" @click="tkConfirm" >
							<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnTit" style="z-index: 999;">确定</view>
						</button>
					</view>
				</view>
				
				<view v-if="currentSubject.type&&currentSubject.type==4">
					<view class="textSplit mb-4 position-relative pt-5">
						<view class="position-absoluteXY"><image src="../../static/images/anti-img5.png" mode=""></image></view>
						<view class="cont font-40 d-flex a-start flex-wrap text-center font-weight">
							<view class="item" :data-index="index" @click="!hasResult?xzChoose($event.currentTarget.dataset.index):''" 
							v-for="(item,index) in textData" :key="index">{{item}}</view>
						</view>
					</view>
					<view class="submitbtn pt-5 mt-3" v-if="!hasResult">
						<button class="btn mb-3" type="button" @click="xzConfirm()" >
							<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnTit" style="z-index: 999;">确定</view>
						</button>
					</view>
				</view>
				
				<view v-if="currentSubject.type&&currentSubject.type==5">
					<view class="mx-4 pt-5">
						<view class="d-flex a-center j-sb flex-wrap seltImgBox">
							<view class="item mb-2" v-for="(item,index) in currentSubject.Option" :key="index">
								<view class="cont d-flex flex-column a-center" :data-index="index" 
					@click="!hasResult?answer($event.currentTarget.dataset.index):''">
									<view class="img"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
									<view class="seltIcon mt-2" v-if="chooseIndex==index&&chooseIndex!=rightIndex">
										<image src="../../static/images/anti-icon9.png" mode=""></image>
									</view>
									<view class="seltIcon mt-2" v-if="index==rightIndex">
										<image src="../../static/images/anti-icon8.png" mode=""></image>
									</view>
									<!-- <view class="seltIcon mt-2">
										<image src="../../static/images/the-orderl-icon5.png" mode=""></image>
									</view> -->
								</view>
							</view>
						</view>
					</view>
				</view>
				
				<!-- <view class="submitbtn mt-5 pt-3">
					<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/buildUp-Answer-multiSelt/buildUp-Answer-multiSelt'}})">
						<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
						<view class="btnTit">下一题</view>
					</button>
				</view> -->
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
				Utils:this.$Utils,
				logData:{},
				is_zan:false,
				textData:[],
				currentSubject:{},
				currentIndex:0,
				hasResult:false,
				right:false,
				time:0,
				//单选参数
				chooseIndex:-1,
				rightIndex:-1,
				//多选参数
				chooseArray:[],
				rightArray:[],
				//填空参数
				text:'',
				score:0,
				statusBar: app.globalData.statusBar,
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
			self.subjectArray = uni.getStorageSync('subjectData');
			console.log('self.subjectArray',self.subjectArray);
			if(options.curr){
				self.currentIndex = parseInt(options.curr)
			};
			if(options.score){
				self.score = parseInt(options.score)
			};
			self.currentSubject = self.subjectArray[self.currentIndex];
			if(self.currentSubject.type==4){
				for (var i = 0; i < self.currentSubject.Option.length; i++) {
					if(self.currentSubject.Option[i].answer==0){
						self.textData = self.currentSubject.Option[i].option.split('')
					}
				}
			};
			if(self.currentSubject.time>0){
				self.time = self.currentSubject.time;
				self.interval = setInterval(function() {
					self.time--; //每执行一次让倒计时秒数减一
					//如果当秒数小于等于0时 停止计时器 
					if (self.time <= 0) {
						clearInterval(self.interval)
						self.logAdd()
					}
				}, 1000);
			}
		},
		
		onUnload() {
			const self = this;
			clearInterval(self.interval);
		},
		
		onHide() {
			const self = this;
			clearInterval(self.interval);
		},
		
		methods: {
			
			previewImage(index) {
				const self = this;
				var imageList = [];
				var current = self.currentSubject.mainImg[index].url;
				for (var i = 0; i < self.currentSubject.mainImg.length; i++) {
					imageList.push(self.currentSubject.mainImg[i].url)
				}
				uni.previewImage({
					current: current,
					urls: imageList
				})
			},
		
			//单选题答题逻辑
			answer(index){
				const self = this;
				console.log(22)
				self.chooseIndex = index;
				for (var i = 0; i < self.currentSubject.Option.length; i++) {
					if(self.currentSubject.Option[i].answer==1){
						self.rightIndex = i
					}
				};
				if(self.chooseIndex==self.rightIndex){
					self.right = true
				}else{
					self.right = false
				};
				self.hasResult = true;
				if(self.currentSubject.time>0){
					clearInterval(self.interval)
				}
				self.logAdd()
				
			},
			
			/* answerConfirm(){
				const self = this;
				
			}, */
			
		//多选答题逻辑
			//选择待提交选项
			dxChoose(index){
				const self = this;
				console.log('index',index)
				var options = self.$Utils.inArray(index,self.chooseArray);
				console.log('options',options)
				if(options>=0){
					console.log('23',23)
					self.chooseArray.splice(options,1)
				}else{
					self.chooseArray.push(index)
				}
				console.log('self.chooseArray',self.chooseArray)
			},
			
			//提交选项
			dxConfirm(){
				const self = this;
				for (var i = 0; i < self.currentSubject.Option.length; i++) {
					if(self.currentSubject.Option[i].answer==1){
						self.rightArray.push(i)
					}
				};
				if(self.$Utils.arrayIsEquals(self.chooseArray,self.rightArray)){
					self.right = true
				}else{
					self.right = false
				};
				self.hasResult = true;
				if(self.currentSubject.time>0){
					clearInterval(self.interval)
				}
				self.logAdd()
			},
			
			//填空题提交答案
			tkConfirm(){
				const self = this;
				self.rightArray = [];
				var textArray = self.text.split(/[(\r\n)\r\n]+/);
				for (var i = 0; i < self.currentSubject.Option.length; i++) {
					if(self.currentSubject.Option[i].answer==1){
						self.rightArray.push(self.currentSubject.Option[i].option)
					}
				};
				console.log(self.rightArray)
				console.log(textArray)
				if(self.rightArray.toString()==textArray.toString()){
					self.right = true
				}else{
					self.right = false
				};
				self.hasResult = true;
				if(self.currentSubject.time>0){
					clearInterval(self.interval)
				}
				self.logAdd()
			},
			
		//选字题逻辑
			//选择待提交选项
			xzChoose(index){
				const self = this;
				self.text += self.textData[index]
			},
			
			//选字提交答案
			xzConfirm(){
				const self = this;
				self.rightText = '';
				//var textArray = self.text.split(/[(\r\n)\r\n]+/);
				for (var i = 0; i < self.currentSubject.Option.length; i++) {
					if(self.currentSubject.Option[i].answer==1){
						self.rightText += self.currentSubject.Option[i].option
					}
				};
				console.log(self.rightText)
				console.log(self.text)
				if(self.rightText ==self.text){
					self.right = true
				}else{
					self.right = false
				};
				self.hasResult = true;
				if(self.currentSubject.time>0){
					clearInterval(self.interval)
				}
				self.logAdd()
			},
			
			deleteText(){
				const self = this;
				self.text = ''
			},
			
			//记录答题结果，跳转下一题
			logAdd() {
				var self = this;
				var postData = {};
				postData.noLoading = true;
				var time = 0;
				var score = self.right?self.currentSubject.score:0;
				time += self.currentSubject.time - self.time;
				self.score += score;
				uni.setStorageSync('score',self.score);
				uni.setStorageSync('time',time);
				postData.tokenFuncName = 'getProjectToken';
				postData.noLoading = true;
				postData.data = {
					type:1,
					relation_table:'Subject',
					relation_id:self.currentSubject.id,
					answer:self.right?1:0,
					score:score,
					sheet_id:uni.getStorageSync('sheetId'),
					behavior:uni.getStorageSync('sheetType')
				};	
			
				if(uni.getStorageSync('sheetType')==1&&self.right){
					postData.saveAfter = [{
						  tableName:'FlowLog',
						  FuncName:'add',
						  data:{
							 type:4,
							 count:score,
							 trade_info:'答题积分',
							 account:1,
							 thirdapp_id:2,
							 user_no:uni.getStorageSync('user_info').user_no
						  }
					}];
				};
				if(uni.getStorageSync('sheetType')==2&&self.right){
					postData.data.time = self.currentSubject.time - self.time
					//postData.data.behavior = 2
				
					postData.saveAfter = [{
						  tableName:'FlowLog',
						  FuncName:'add',
						  data:{
							 type:5,
							 count:score,
							 trade_info:'答题积分',
							 account:1,
							 thirdapp_id:2,
							 user_no:uni.getStorageSync('user_info').user_no
						  }
					}];
				};
				var callback = function(res) {
					if (res && res.solely_code == 100000&&res.info.id) {
						setTimeout(function() {
							self.hasResult = false;
							self.chooseIndex=-1;
							self.rightIndex=-1;
							self.chooseArray = [];
							self.rightArray = [];
							self.textData = [];
							self.right = false;
							self.text = '';
							if(self.currentIndex==self.subjectArray.length-1){
								if(uni.getStorageSync('sheetType')==1){
									self.$Router.redirectTo({route:{path:'/pages/buildUp-Result/buildUp-Result'}});
								}else if(uni.getStorageSync('sheetType')==2){
									self.$Router.redirectTo({route:{path:'/pages/FengYun-Result/FengYun-Result'}});
								}
								return
							};
							self.currentIndex++;
							self.currentSubject = self.subjectArray[self.currentIndex];
							self.logData = {};
							self.getLogData();
							self.time = self.currentSubject.time;
							clearInterval(self.interval);
							self.interval = setInterval(function() {
								self.time--; //每执行一次让倒计时秒数减一
								//如果当秒数小于等于0时 停止计时器 
								if (self.time <= 0) {
									clearInterval(self.interval)
									self.logAdd()
								}
							}, 1000);
							if(self.currentSubject.type==4){
								for (var i = 0; i < self.currentSubject.Option.length; i++) {
									if(self.currentSubject.Option[i].answer==0){
										self.textData = self.currentSubject.Option[i].option.split('')
									}
								}
							};
						}, 1500);
					}
				};
				self.$apis.logAdd(postData, callback);
			},
			
			getLogData() {
				const self = this;
				const postData = {
					searchItem: {
						relation_id: self.currentSubject.id,
						relation_table:'Subject',
						//behavior:1,
						status:['in',[1,-1]],
						user_no:uni.getStorageSync('user_info').user_no,
						type:2
					},
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						if(res.info.data.length>0){
							self.logData = res.info.data[0]
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logGet(postData, callback);
			},
			
			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (!self.logData.status) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 2,
					title: '点赞成功',
					relation_id: self.currentSubject.id,
					relation_table:'Subject',
					user_no: uni.getStorageSync('user_info').user_no,
					behavior:1
				};
				if(self.currentSubject.battle==1){
					postData.data.behavior = 2
					//postData.saveAfter[0].data.type = 5
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.logData = {
							status: 1,
							id: res.info.id
						};
						
						//self.$Utils.showToast('已收藏', 'none', 1000)
					} else {
						self.$Utils.showToast('点赞失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.logData.id
					},
					data: {
						status: -self.logData.status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.logData.status = -self.logData.status;
						
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	.textSplit{width:635rpx;height: 658rpx;margin: 0 auto;padding: 24rpx 8rpx;}
	.textSplit .cont{position: relative;z-index: 2;}
	.textSplit .item{width: 25%;height: 156rpx;line-height: 156rpx;}
</style>
