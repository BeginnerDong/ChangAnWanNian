<template>
	<view style="height: 100%;">
		<pageBj></pageBj>

		<view class="Box">

			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar+'px'}">
				<view class="headBj">
					<image src="../../static/images/head-img.png" mode=""></image>
				</view>
				<view class="tit">关于我们</view>
			</view>
			<view class="pageBox" :style="{marginTop:statusBar+44 + 'px'}">
				<view class="mx-3">
					<view class="xqInfor">
						<view class="cont font-26">
							<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
							</view>
						</view>
					</view>
					<view class="submitbtn mt-4">
						<button class="btn" type="button" 
						@click="Router.navigateTo({route:{path:'/pages/Online-ProListDetail/Online-ProListDetail?id=25'}})">
							<view class="btnBj">
								<image src="../../static/images/anti-icon.png" mode=""></image>
							</view>
							<view class="btnTit">赞赏本文</view>
						</button>
					</view>
				</view>
				
				<!--底部tab键-->
				<view class="navbar">
					<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
						<view class="nav_img">
							<image src="../../static/images/nabar1.png" />
						</view>
						<view class="text">首页</view>
					</view>
					<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})">
						<view class="nav_img">
							<image src="../../static/images/nabar2-a.png" />
						</view>
						<view class="text this-text">关于我们</view>
					</view>
					<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
						<view class="nav_img">
							<image src="../../static/images/nabar3.png" />
						</view>
						<view class="text">我的</view>
					</view>
				</view>
				<view style="height: 100rpx;width: 100%;"></view>
				<!--底部tab键 end-->
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
				mainData: {},
				statusBar: app.globalData.statusBar,
			}
		},

		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2
				};
				postData.getBefore = {
					article: {
						tableName: 'Label',
						middleKey: 'menu_id',
						key: 'id',
						searchItem: {
							title: ['in', ['关于我们']],
						},
						condition: 'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";

	.pageBox {
		margin-bottom: 110rpx;
	}
</style>
