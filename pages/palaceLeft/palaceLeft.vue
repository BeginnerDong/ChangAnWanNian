<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">宫殿</view>
			</view>
		<view class="pageBox" :style="{top:statusBar + 'px'}">
			<view class="palaceBox position-relative text-center">
				<view class="position-absoluteXY"><image class="w" src="../../static/images/palaceBox-img1.png" mode="widthFix"></image></view>
				<view class="cont font-30">
					<view class="box1 d-flex a-center flex-wrap">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=修真'}})">修真</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=安定'}})">安定</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=修德'}})">修德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=普宁'}})">普宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=修祥'}})">修祥</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=辅兴'}})">辅兴</view>
					</view>
					<view class="box2 d-flex a-center flex-wrap">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=义宁'}})">义宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=金城'}})">金城</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=颁政'}})">颁政</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=居德'}})">居德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=醴泉'}})">醴泉</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=布政'}})">布政</view>
					</view>
					<view class="box3 d-flex a-center flex-wrap">
						<view class="split">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=群贤'}})">群贤</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=怀德'}})">怀德</view>
						</view>
						<view class="split M">
							<view class="child position-relative font-36" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=西市'}})"><image style="width:60rpx;height:40rpx;position: absolute;top: 0rpx;left: 20rpx;" src="../../static/images/palaceBox-icon1.png" mode=""></image>西市</view>
						</view>
						<view class="split">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=延寿'}})">延寿</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=光德'}})">光德</view>
						</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=崇化'}})">崇化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=怀远'}})">怀远</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=延康'}})">延康</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=丰邑'}})">丰邑</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=长寿'}})">长寿</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=崇贤'}})">崇贤</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=待贤'}})">待贤</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=嘉会'}})">嘉会</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=延福'}})">延福</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永和'}})">永和</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永平'}})">永平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永安'}})">永安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=常安'}})">常安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=通轨'}})">通轨</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=敦义'}})">敦义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=和平'}})">和平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=归义'}})">归义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=大通'}})">大通</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永阳'}})">永阳</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=昭行'}})">昭行</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=大安'}})">大安</view>
					</view>
					
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
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				statusBar:app.globalData.statusBar
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
