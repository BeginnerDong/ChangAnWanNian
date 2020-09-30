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
			<view class="tit">电商文创</view>
		</view>
		<view class="f5bj  flexX topNavFix" :style="{marginTop:statusBar+44 + 'px'}">
			<view class="orderNav flexX bg-white  shadow color6">
				<view class="tt flex-shrink" @click="change(-1)"
				:class="curr==-1?'on':''">全部</view>
				<view class="tt flex-shrink" @click="change(index)"
				:class="curr==index?'on':''" 
				v-for="(item,index) in labelData" :key="index">{{item.title}}</view>
			</view>
		</view>
		<scroll-view scroll-y="true" class="pageBox pb-4"  @scrolltolower="Bottom"   
		:style="{marginTop:statusBar+84 + 'px'}">
			<view class="topNavH"></view>
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item rounded10 mb-3" v-for="(item,index) in mainData" :key="index" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/Online-ProListDetail/Online-ProListDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit avoidOverflow2 font-30">{{item.title}}</view>
						<view class="price font-30 font-weight mt-2 d-flex a-center">
							<view class="priceIcon">
								<image src="../../static/images/about-img2.png" mode=""></image>
							</view>
							<view>{{item.price}}</view>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0">
				<image src="../../static/images/nodata.png" mode=""></image>
			</view>
		</scroll-view>

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
				mainData: [],
				statusBar: app.globalData.statusBar,
				labelData:[],
				curr:-1,
				width:'',
				searchItem:{
					thirdapp_id:2,
					on_shelf:1
				},
				idArray:[]
			}
		},

		onLoad(options) {
			const self = this;
			if(options.num){
				self.num = options.num
			};
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
		},

		

		methods: {
			
			change(num){
				const self = this;
				if(num!=self.curr){
					self.curr = num;
					if(self.curr==-1){
						self.searchItem.category_id = ['in',self.idArray]
					}else{
						self.searchItem.category_id = self.labelData[self.curr].id
					};
					self.getMainData(true)
				}
			},
				
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.order = {
					listorder: 'desc'
				};
				postData.getBefore= {
					child:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							status:['in',1],
							title:['in','电商文创']
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						};
						if(self.num){
							self.curr = self.num;
							self.searchItem.category_id = self.labelData[self.curr].id
						}else{
							self.searchItem.category_id = ['in',self.idArray]
						}
						self.getMainData()
						self.width = 100/(self.labelData.length+1)+'%'
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			Bottom() {
				console.log('onReachBottom')
				const self = this;
				if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
					self.paginate.currentPage++;
					self.getMainData()
				};
			},

			navigateBack() {
				uni.navigateBack({});
			},

			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				/* if(!self.searchItem.category_id){
					postData.getBefore = {
						article: {
							tableName: 'Label',
							middleKey: 'category_id',
							key: 'id',
							searchItem: {
								title: ['in', ['电商文创']],
							},
							condition: 'in'
						}
					};
				}; */
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/productList.css";

	page {
		background-color: #F5F5F5;
	}
	
	.topNavFix {
		width: 100%;
		height: 80rpx;
		position: fixed;
		right: 0;
		left: 0;
		box-sizing: border-box;
		z-index: 22;
	}
	
	.topNavH {
		height: 50rpx;
	}
	.orderNav{width: 100%;}
	.orderNav .tt{width: 22%}
</style>
