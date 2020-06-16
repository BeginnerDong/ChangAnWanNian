<template>
	<view>
		<pageBj></pageBj>


		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">探索穿越</view>
		</view>
		<scroll-view scroll-y="true" class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="px-3">
				<view class="font-30 font-weight mt-1">唐长安城地图</view>
				<view class="map mt-2">
					<view class="mapInfor d-flex a-center">
						<!-- 宫殿左边 -->
						<view class="item" style="width: 260rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceLeft/palaceLeft'}})">
							<image src="../../static/images/explorel-img2.png" mode=""></image>
						</view>
						<!-- 宫殿中间 -->
						<view class="item" style="width: 184rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceMiddle/palaceMiddle'}})">
							<image src="../../static/images/explorel-img3.png" mode=""></image>
						</view>
						<!-- 宫殿右边 -->
						<view class="item" style="width: 246rpx;" @click="Router.navigateTo({route:{path:'/pages/palaceRight/palaceRight'}})">
							<image src="../../static/images/explorel-img4.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="proRow prolist mt-4">
					<view class="item d-flex j-sb  rounded10 mb-4" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
					 @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="tit avoidOverflow2 font-28">{{item.title}}</view>
							<view class="B-price">
								<view class="font-24 color9">{{item.create_time}}</view>
							</view>
						</view>
					</view>
				</view>

				<!-- 无数据 -->
				<view class="nodata" v-if="mainData.length==0">
					<image src="../../static/images/nodata.png" mode=""></image>
				</view>
				
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

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},

		methods: {

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
						middleKey: 'menu_id',
						key: 'id',
						searchItem: {
							title: ['in', ['探索穿越']],
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
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";

	page {
		padding-bottom: 40rpx;
	}

	.swiper-box {
		height: 280rpx;
		box-sizing: border-box;
		border-radius: 10rpx;
		overflow: hidden;
	}

	/* .swiper-box swiper-item{width: 100%;box-sizing: border-box;} */

	/* 分类导航 */
	.indHome .item {
		flex: 1;
		text-align: center;
		color: #222;
		display: flex;
		flex-direction: column;
		margin-bottom: 40rpx;
		line-height: 36rpx;
	}

	.indHome .item image {
		width: 111rpx;
		height: 112rpx;
		margin: 0 auto 16rpx auto;
	}

	.mapInfor {
		width: 100%;
		height: 360rpx;
	}

	.mapInfor .item {
		overflow: hidden;
		height: 360rpx;
	}
</style>
