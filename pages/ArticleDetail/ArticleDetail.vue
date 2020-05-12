<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">详情</view>
			</view>
			<view class="">
				<view class="font-weight font-34 mx-3 ">考古学家利用激光揭露100公里玛雅石路之谜石路之谜</view>
				<view class="d-flex j-end a-center mt-1" @click="zan">
					<view class="d-flex j-center a-center zanBtn">
						<view class="icon mr-1">
							<image v-show="!is_zan" src="../../static/images/explorel-icon1.png" mode=""></image>
							<image v-show="is_zan" src="../../static/images/explorel-icon2.png" mode=""></image>
						</view>
						<view class="font-26 color6">点赞</view>
					</view>
				</view>
				<view class="xqInfor mx-3 pt-1">
					<view class="cont font-26">
						<view><image class="w" src="../../static/images/explorel-img1.png" mode="widthFix"></image></view>
						<view>还罚款四氯化硅过会就克劳福德水果零食给扔了供货商挂号费就开始李国峰挂号费数据库打了个个净空法师的火锅后方可吉林省很尴尬规划局开发商搞弗兰克斯挂号费就开始和公交卡发十个开发低功耗规划局开发单身公害</view>
						<view>还罚款四氯化硅过会就克劳福德水果零食给扔了供货商挂号费就开始李国峰挂号费数据库打了个个净空法师的火锅后方可吉林省很尴尬规划局开发商搞弗兰克斯挂号费就开始和公交卡发十个开发低功耗规划局开发单身公害</view>
						<view>还罚款四氯化硅过会就克劳福德水果零食给扔了供货商挂号费就开始李国峰挂号费数据库打了个个净空法师的火锅后方可吉林省很尴尬规划局开发商搞弗兰克斯挂号费就开始和公交卡发十个开发低功耗规划局开发单身公害</view>
						
					</view>
				</view>
			</view>
			
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
				wx_info:{},
				is_zan:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack({
				});
			},
			zan(){
				const self = this;
				self.is_zan = !self.is_zan
			}

		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 40rpx;}
	
	.zanBtn{width: 140rpx;height: 60rpx;border-radius: 30rpx 0 0 30rpx;background-color: #f5e8d6;}
	.zanBtn .icon{width: 34rpx;height: 34rpx;}
</style>
