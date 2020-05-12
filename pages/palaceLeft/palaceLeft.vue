<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">宫殿</view>
			</view>
			<view class="palaceBox position-relative text-center">
				<view class="position-absoluteXY"><image class="w" src="../../static/images/palaceBox-img1.png" mode="widthFix"></image></view>
				<view class="cont font-30">
					<view class="box1 d-flex a-center flex-wrap">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">修真</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安定</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">修德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">普宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">修祥</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">辅兴</view>
					</view>
					<view class="box2 d-flex a-center flex-wrap">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">义宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">金城</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">颁政</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">居德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">醴泉</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">布政</view>
					</view>
					<view class="box3 d-flex a-center flex-wrap">
						<view class="split">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">群贤</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">怀德</view>
						</view>
						<view class="split M">
							<view class="child position-relative font-36" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})"><image style="width:60rpx;height:40rpx;position: absolute;top: 0rpx;left: 20rpx;" src="../../static/images/palaceBox-icon1.png" mode=""></image>西市</view>
						</view>
						<view class="split">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">延寿</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">光德</view>
						</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">崇化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">怀远</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">延康</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">丰邑</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">长寿</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">崇贤</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">待贤</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">嘉会</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">延福</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永和</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">常安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">通轨</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">敦义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">和平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">归义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">大通</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永阳</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">昭行</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">大安</view>
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
				wx_info:{}
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
			}

		},
	};
</script>

<style>
	page{padding-bottom: 40rpx;}
	.palaceBox{padding: 146rpx 38rpx 0 144rpx;width: 100%;height: 1692rpx;}
	.palaceBox .cont{position: relative;z-index: 2;}
	.palaceBox .child{width: 176rpx;height: 94rpx;border: 1px solid #000000;display: flex;align-items: center; justify-content: center; margin: 0 20rpx 20rpx 0;background-color: rgba(255, 255, 255, 0.7);}
	.palaceBox .child:nth-of-type(3n){margin-right: 0;}
	.palaceBox .child:hover{background: #D0B487; color: #fff;border: 1px solid #9f8253;}
	
	.palaceBox .box2{ padding-top: 16rpx;}
	.palaceBox .box2 .child{height: 126rpx; margin-bottom: 30rpx;}
	
	/* .palaceBox .box3{ padding-top:10rpx;} */
	.palaceBox .box3 .child{height: 88rpx; margin-bottom: 16rpx;}
	.palaceBox .split{width: 176rpx;margin-right: 20rpx;height: 180rpx;}
	.palaceBox .split .child{height: 76rpx;}
	.palaceBox .split:nth-of-type(3n){margin-right: 0;}
	.palaceBox .split.M .child{height: 164rpx;}
</style>
