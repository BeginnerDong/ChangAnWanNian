<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		<view class="Box">
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				
				<view class="tit">题目预览</view>
			</view>
			
			<view class="pageBox pb-4" v-if="hasSearch" :style="{marginTop:statusBar+ 44 + 'px'}">	
				<view class="d-flex j-center a-center" style="margin-top: 80rpx;">
					<view class="answerTime position-relative text-center">
						<view class="position-absoluteXY"><image src="../../static/images/anti-img.png" mode=""></image></view>
						<view class="infor pt-2">
							<view class="font-24 color6">
								<span class="color2 font-weight mr-1" style="font-size: 50rpx;">{{Utils.numToChinese(currentIndex+1)}}</span>
								<span v-if="currentSubject.type==1||currentSubject.type==5">单选</span>
								<span v-if="currentSubject.type==2">多选</span>
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
						<view class="zanBtn mr-1">
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
							<image v-if="!hasResult&&text.length>0"  class="d-inline-block" style="width: 50rpx;height:50rpx;position: absolute;right: 0;"
							src="../../static/images/anti-icon10.png" mode=""></image>
						</view>
					</view>
				</view>
				
				<view class="submitbtn pt-5" v-if="currentSubject.type&&currentSubject.type==1">
					<button class="btn mb-3"  v-for="(item,index) in currentSubject.Option" :key="index" 
					type="button">
						<view class="seltIconL errorIcon" v-if="chooseIndex==index&&chooseIndex!=rightIndex"><image src="../../static/images/anti-icon4.png" mode=""></image></view>
						<view class="btnBj" v-if="chooseIndex==index&&chooseIndex!=rightIndex"><image src="../../static/images/anti-icon1.png" mode=""></image></view>
						<view class="btnBj" v-if="chooseIndex!=index&&index!=rightIndex"><image src="../../static/images/anti-icon.png" mode=""></image></view>
						<view class="btnBj" v-if="index==rightIndex"><image src="../../static/images/anti-icon2.png" mode=""></image></view>
						<view class="btnTit color2" style="z-index: 999;">{{item.option}}</view>
						<view class="seltIconR rightIcon" v-if="index==rightIndex"><image src="../../static/images/anti-icon3.png" mode=""></image></view>
					</button>
				</view>
				
				<view  v-if="currentSubject.type&&currentSubject.type==2">
					<view class="submitbtn pt-5">
						<button class="btn mb-3"  v-for="(item,index) in currentSubject.Option" :key="index" type="button">
							<view class="seltIconL errorIcon" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)<0"><image src="../../static/images/anti-icon4.png" mode=""></image></view>
							<view class="btnBj" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)<0"><image src="../../static/images/anti-icon1.png" mode=""></image></view>
							<view class="btnBj" v-if="!hasResult||(hasResult&&Utils.inArray(index,chooseArray)<0&&Utils.inArray(index,rightArray)<0)"><image src="../../static/images/anti-icon.png" mode=""></image></view>
							<view class="btnBj" v-if="hasResult&&Utils.inArray(index,rightArray)>=0"><image src="../../static/images/anti-icon2.png" mode=""></image></view>
							<view class="btnTit color2" style="z-index: 999;" :class="Utils.inArray(index,chooseArray)>=0?'colorRed':''">{{item.option}}</view>
							<view class="seltIconR rightIcon" v-if="hasResult&&Utils.inArray(index,chooseArray)>=0&&Utils.inArray(index,rightArray)>=0"><image src="../../static/images/anti-icon3.png" mode=""></image></view>
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
					
				</view>
				
				<view v-if="currentSubject.type&&currentSubject.type==4">
					<view class="textSplit mb-4 position-relative pt-5">
						<view class="position-absoluteXY"><image src="../../static/images/anti-img5.png" mode=""></image></view>
						<view class="cont font-40 d-flex a-start flex-wrap text-center font-weight">
							<view class="item" 
							v-for="(item,index) in textData" :key="index">{{item}}</view>
						</view>
					</view>
					
				</view>
				
				<view v-if="currentSubject.type&&currentSubject.type==5">
					<view class="mx-4 pt-5">
						<view class="d-flex a-center j-sb flex-wrap seltImgBox">
							<view class="item mb-2" v-for="(item,index) in currentSubject.Option" :key="index">
								<view class="cont d-flex flex-column a-center">
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
				
				<view class="submitbtn mt-5 pt-3">
					<button class="btn" type="button" @click="reSearch">
						<view class="btnBj"><image src="../../static/images/anti-icon.png" mode=""></image></view>
						<view class="btnTit">重新搜索</view>
					</button>
				</view>
				
				<view class="answerTit px-4 py-5 font-34 mt-2">
					<view>解析</view>
					
					<view class="content ql-editor" style="padding:0;" v-html="currentSubject.content">
					</view>
				</view>
			</view>
			<view class="pageBox pb-4" :style="{marginTop:statusBar+ 44 + 'px'}" v-if="!hasSearch">
				<view class="loginCont">
					<view class="item d-flex a-center px-2">
						
						<view class="input">
							<input type="number" v-model="subjectId" placeholder="输入题目ID搜索题目预览" placeholder-class="placeholder">
						</view>
						<view class="xian">
							<image src="../../static/images/the-loginl-icon2.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="submitbtn pdtb15" style="margin-top:100rpx;">
					<button class="btn" type="button" @click="subjectGet">
						<view class="btnBj">
							<image src="../../static/images/buttonl-icon.png" mode=""></image>
						</view>
						<view class="btnTit">搜索</view>
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
				subjectId:'',
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
				hasSearch:false
			}
		},
		
		onLoad(options) {
			const self = this;
			
		},
		
		
		methods: {
			
			reSearch(){
				const self = this;
				self.hasSearch = false;
				self.subjectId = '';
			},
			
			subjectGet() {
				var self = this;
				if(self.subjectId==''){
					self.$Utils.showToast('请输入ID搜索', 'none');
					return
				};
				var postData = {};
				postData.searchItem = {
					id:self.subjectId
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.hasSearch = true;
						self.currentSubject = res.info.data[0];
						self.time = self.currentSubject.time;
						if(self.currentSubject.type==4){
							for (var i = 0; i < self.currentSubject.Option.length; i++) {
								if(self.currentSubject.Option[i].answer==0){
									self.textData = self.currentSubject.Option[i].option.split('')
								}
							}
						}; 
					}else{
						self.$Utils.showToast('未搜索到对应题目', 'none');
					}
				};
				self.$apis.subjectGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/answer.css";
	.textSplit{width:635rpx;height: 658rpx;margin: 0 auto;padding: 24rpx 8rpx;}
	.textSplit .cont{position: relative;z-index: 2;}
	.textSplit .item{width: 25%;height: 156rpx;line-height: 156rpx;}
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
