<template>
	<view style="height: 100%;">

		<pageBj></pageBj>

		<view class="Box">
		<view class="page-head d-flex a-center j-center"  :style="{marginTop:statusBar+'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">周末讲堂</view>
		</view>
		<view  @scrolltolower="Bottom" class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item mb-3" v-for="(item,index) in mainData" :key="index" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/week-lecture-Detail/week-lecture-Detail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic rounded10">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor avoidOverflow font-30">{{item.title}}</view>
				</view>
			</view>

			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0">
				<image src="../../static/images/nodata.png" mode=""></image>
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
				Router: this.$Router,
				mainData: [],
				statusBar: app.globalData.statusBar,
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
				postData.searchItem = {
					thirdapp_id: 2,
				}
				postData.getBefore = {
					article: {
						tableName: 'Label',
						middleKey: 'category_id',
						key: 'id',
						searchItem: {
							title: ['in', ['周末讲堂']],
						},
						condition: 'in'
					}
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

	.productList .item {
		height: auto;
		background-color: initial;
		border-radius: 0;
	}

	.productList .item .pic {
		height: 220rpx;
		overflow: hidden;
	}

	.productList .item .infor {
		padding: 10rpx 0 0 0;
	}
</style>
