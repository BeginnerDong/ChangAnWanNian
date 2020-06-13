<template>
	<view>
		<pageBj></pageBj>
		
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">宫殿</view>
			</view>
			<view class="pageBox">
			<view class="palaceBox position-relative text-center">
				<view class="position-absoluteXY"><image class="w" src="../../static/images/palaceBox-img3.png" mode="widthFix"></image></view>
				<view class="cont font-30">
					<view class="box1 d-flex a-center flex-wrap">
						<view class="child halfy">
							<view class="child L"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=光宅'}})">光宅</view>
							<view class="child R"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=翊善'}})">翊善</view>
						</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=长乐'}})">长乐</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=十六王宅（永福坊）'}})">十六王宅（永福坊）</view>
						<view class="child halfy">
							<view class="child L"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永昌'}})">永昌</view>
							<view class="child R"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=来庭'}})">来庭</view>
						</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=大宁'}})">大宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=兴宁'}})">兴宁</view>
					</view>
					<view class="box2 d-flex a-start flex-wrap ">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永兴'}})">永兴</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=安兴'}})">安兴</view>
						<view class="child" style="height: 72rpx;" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永嘉'}})">永嘉</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=崇仁'}})">崇仁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=胜业'}})">胜业</view>
						<view class="child" style="margin-top: -72rpx;height: 216rpx;opacity: 0; margin-bottom: 0;" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=兴庆宫'}})">兴庆宫</view>
					</view>
					<view class="box3 d-flex a-center flex-wrap">
						<view class="split">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=平康'}})">平康</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=宣阳'}})">宣阳</view>
						</view>
						<view class="split M">
							<view class="child position-relative font-36" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=东市'}})"><image style="width:60rpx;height:40rpx;position: absolute;top: 0rpx;left: 20rpx;" src="../../static/images/palaceBox-icon1.png" mode=""></image>东市</view>
						</view>
						<view class="split R">
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=道政'}})">道政</view>
							<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=常乐'}})">常乐</view>
						</view>
						
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=亲仁'}})">亲仁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=安邑'}})">安邑</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=靖恭'}})">靖恭</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永宁'}})">永宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=宣平'}})">宣平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=新昌'}})">新昌</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=永崇'}})">永崇</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=升平'}})">升平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=升道'}})">升道</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=昭国'}})">昭国</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=修行'}})">修行</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=立政'}})">立政</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=晋昌'}})">晋昌</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=修政'}})">修政</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=敦化'}})">敦化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=通善'}})">通善</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=青龙'}})">青龙</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages//'}})"></view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=通济'}})">通济</view>
						<view class="child" style="width: 120rpx;" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail?name=曲池'}})">曲池</view>
					</view>
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	import pageBj from 'components/pageBj/pageBj';
	export default {
		components: {
			pageBj
		},
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
	.palaceBox{padding: 626rpx 172rpx 0 58rpx;width: 100%;height: 2300rpx;}
	.palaceBox .cont{position: relative;z-index: 2;}
	.palaceBox .child{width: 140rpx;height: 100rpx;border: 1px solid #000000;display: flex;align-items: center; justify-content: center; margin: 0 20rpx 20rpx 0;background-color: rgba(255, 255, 255, 0.7);}
	.palaceBox .child:nth-of-type(3n-1){width: 164rpx;}
	.palaceBox .child:nth-of-type(3n){width: 176rpx;margin-right: 0;}
	.palaceBox .child:hover{background: #D0B487; color: #fff;border: 1px solid #9f8253;}
	.palaceBox .child.on{background: #D0B487; color: #fff;border: 1px solid #9f8253;}
	
	.palaceBox .halfy{border: 0;text-align: center;font-size: 26rpx;}
	.palaceBox .halfy .child{margin: 0;}
	.palaceBox .halfy .L{width: 68rpx;margin-right: 16rpx;}
	.palaceBox .halfy .R{width: 54rpx;}
	
	.palaceBox .box2{ padding-top: 16rpx;}
	.palaceBox .box2 .child{height: 128rpx; margin-bottom: 30rpx;}
	
	/* .palaceBox .box3{ padding-top:10rpx;} */
	.palaceBox .box3 .child{height: 92rpx; margin-bottom: 16rpx;}
	
	
	.palaceBox .split{width: 140rpx;margin-right: 20rpx;}
	
	.palaceBox .split .child{height: 76rpx;margin-right: 0;}
	.palaceBox .split .child:nth-of-type(2n){width: auto;}
	.palaceBox .split:nth-of-type(3n){margin-right: 0;}
	.palaceBox .split.M{width: 164rpx; margin-right: 20rpx;}
	.palaceBox .split.M .child{width: 164rpx;height: 172rpx;margin-right: 0;}
	.palaceBox .split.R{width: 176rpx; margin-right: 0rpx;}
	.palaceBox .split.R .child{width: 100%;}
</style>
