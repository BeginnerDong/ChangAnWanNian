<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">关于我们</view>
			</view>
			<view class="mx-3">
				<view class="xqInfor">
					<view class="cont font-26">
						<view>还罚款四氯化硅过会就克劳福德水果零食给扔了供货商挂号费就开始李国峰挂号费数据库打了个个净空法师的火锅后方可吉林省很尴尬规划局开发商搞弗兰克斯挂号费就开始和公交卡发十个开发低功耗规划局开发单身公害</view>
						<view>还罚款四氯化硅过会就克劳福德水果零食给扔了供货商挂号费就开始李国峰挂号费数据库打了个个净空法师的火锅后方可吉林省很尴尬规划局开发商搞弗兰克斯挂号费就开始和公交卡发十个开发低功耗规划局开发单身公害</view>
						<view><image class="w" src="../../static/images/about-usl-img.png" mode="widthFix"></image></view>
					</view>
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
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/aboutUs/aboutUs'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar2-a.png" />
					</view>
					<view class="text this-text">关于我们</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar3.png" />
					</view>
					<view class="text">我的</view>
				</view>
			</view>
			<!--底部tab键 end-->
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj'
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 60rpx;}
	
	
</style>
