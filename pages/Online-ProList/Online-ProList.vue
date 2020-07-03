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
		<scroll-view scroll-y="true" class="pageBox pb-4"  @scrolltolower="Bottom"   :style="{marginTop:statusBar+44 + 'px'}">
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
				statusBar: app.globalData.statusBar
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
				postData.searchItem = {
					thirdapp_id: 2,
				}
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
</style>
