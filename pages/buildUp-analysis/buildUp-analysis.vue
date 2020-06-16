<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">题目解析</view>
			</view>
		<scroll-view scroll-y="true" @scrolltolower="Bottom" class="pageBox" :style="{top:statusBar + 'px'}">	
			<view class="answerList" v-for="(item,index) in mainData" :key="index">
				<view class="mx-3">
					<view>{{index+1}}:{{item.title}}</view>
					<view class="answerBj px-3 d-flex a-start py-2 mt-2 ">
						<view class="color6 mr-5 ">答案</view>
						<view class="mr-2" v-for="(c_item,c_index) in item.Option" v-if="c_item.answer==1&&c_item.option!=''">
							{{c_item.option}}
						</view>
						<view class="answerImg" v-for="(c_item,c_index) in item.Option" v-if="c_item.answer==1&&c_item.mainImg.length!=0">
							<image :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:''" mode=""></image>
						</view>
					</view>
					<view class="jiexiTit mt-3 font-weight">题目解析</view>
					<view class="color6 my-2">
						<view class="content ql-editor" style="padding:0;" v-html="item.content">
						</view>
					</view>
				</view>
				<view class="f5Bj-H20 mb-3"><image src="../../static/images/home-icon4.png" mode=""></image></view>
			</view>
			
			<view style="height: 260rpx;width: 100%;"></view>
		</scroll-view>
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
				showView: false,
				score:'',
				wx_info:{},
				answerData:3,
				mainData:[],
				statusBar: app.globalData.statusBar,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.type=options.type;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self)
			/* if(options.type==1){
				self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				self.$Utils.loadAll(['getMainData'], self)
			}else if(options.type==3){
				self.mainData = uni.getStorageSync('subjectData');
			}; */
			//self.$Utils.loadAll(['getMainData'], self)
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
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:1,
					relation_table:'Subject',
					behavior:1,
				};
				if(self.type==3){
					postData.searchItem.sheet_id = uni.getStorageSync('sheetId')
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
						for (var i = 0; i < res.info.data.length; i++) {
							self.mainData.push(res.info.data[i].subject[0])
						}
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},

		},
	};
</script>

<style>
	page{padding-bottom: 100rpx;}
	.answerList:last-child .f5Bj-H20{display: none;}
	.answerBj{background-color: #f5e8d6;}
	.jiexiTit{position: relative;padding-left: 20rpx;}
	.jiexiTit::before{content: "";width: 8rpx;height: 24rpx;background-color: #D0B487;position: absolute;left: 0;top: 6rpx;}
	.answerImg{width: 150rpx;height: 150rpx;}
</style>
