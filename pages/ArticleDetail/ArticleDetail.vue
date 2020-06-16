<template>
	<view style="height: 100%;">
		<pageBj></pageBj>

		<view class="Box">

		<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
			<view class="backBtn" @click="Router.back(1)">
				<image src="../../static/images/back-icon.png" mode=""></image>
			</view>
			<view class="headBj">
				<image src="../../static/images/head-img.png" mode=""></image>
			</view>
			<view class="tit">详情</view>
		</view>
		<view class="pageBox pb-4" :style="{marginTop:statusBar+44 + 'px'}">
			<view class="" v-if="mainData.title">
				<view class="font-weight font-34 mx-3 ">{{mainData.title}}</view>
				<view class="d-flex j-end a-center mt-1" @click="Utils.stopMultiClick(clickGood)">
					<view class="d-flex j-center a-center zanBtn">
						<view class="icon mr-1">
							<image :src="mainData.log&&mainData.log.length>0&&mainData.log[0].status==1?'../../static/images/explorel-icon2.png':'../../static/images/explorel-icon1.png'"
							 mode=""></image>
						</view>
						<view class="font-26 color6">点赞</view>
					</view>
				</view>
				<view class="xqInfor mx-3 pt-1">
					<view class="cont font-26">
						<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
						</view>
					</view>
				</view>
			</view>
			<view v-else style="text-align: center;line-height: 100px;font-weight: 700;font-size: 18px;">暂 无 数 据！</view>
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
				is_zan: false,
				searchItem: {
					thirdapp_id: 2
				},
				mainData: {},
				Utils: this.$Utils,
				statusBar: app.globalData.statusBar,
			}
		},

		onLoad(options) {
			const self = this;
			if (options.id) {
				self.searchItem.id = options.id
			} else if (options.name) {
				self.searchItem.small_title = options.name
			} else {
				self.$Utils.showToast('数据有误，请重试', 'none');
				setTimeout(function() {
					uni.navigateBack({
						delta: 1
					})
				}, 1000);
			};
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {


			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.mainData.log.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},

			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.mainData.id,
					relation_table: 'Article',
					user_no: uni.getStorageSync('user_info').user_no,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.log.push({
							status: 1,
							id: res.info.id
						});

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
						id: self.mainData.log[0].id
					},
					data: {
						status: -self.mainData.log[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.log[0].status = -self.mainData.log[0].status;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					log: {
						token: uni.getStorageSync('user_token'),
						tableName: 'Log',
						middleKey: 'id',
						key: 'relation_id',
						searchItem: {
							status: ['in', [1, -1]],
							user_no: uni.getStorageSync('user_info').user_no,
							relation_table: 'Article'
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},





		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";


	.zanBtn {
		width: 140rpx;
		height: 60rpx;
		border-radius: 30rpx 0 0 30rpx;
		background-color: #f5e8d6;
	}

	.zanBtn .icon {
		width: 34rpx;
		height: 34rpx;
	}
</style>
