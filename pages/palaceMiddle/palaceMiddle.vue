<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">宫殿</view>
			</view>
			<view style="width: 100%;height: 450rpx;"><image class="w" src="../../static/images/palaceBox-img2.png" mode="widthFix"></image></view>
			
			<!-- 宫城 -->
			<view class="" style="width: 750rpx;height: 308rpx;"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">
				<image src="../../static/images/palaceBox-img2.1.png" mode=""></image>
			</view>
			
			<!-- 皇城 -->
			<view class="" style="width: 750rpx;height: 490rpx;"  @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">
				<image src="../../static/images/palaceBox-img2.2.png" mode=""></image>
			</view>
			
			<view class="palaceBox position-relative text-center">
				<view class="cont font-30">
					<view class="box1 d-flex a-center flex-wrap">
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">太平</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">善和</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">兴道</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">务本</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">通义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">通化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">开化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">崇义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">兴化</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">丰乐</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安仁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">长兴</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">崇德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安业</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">光福</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永乐</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">怀贞</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">崇业</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">靖善</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">靖安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">宣义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">永达</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">兰陵</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安善</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">丰安</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">道德</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">开明</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">大业</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">昌明</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">光兴</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">保宁</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">昌乐</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安乐</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">延祚</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安义</view>
						<view class="child" @click="Router.navigateTo({route:{path:'/pages/ArticleDetail/ArticleDetail'}})">安德</view>
						
					</view>
				</view>
			</view>
			<view class="w" style="height: 84rpx;"><image src="../../static/images/palaceBox-img2.3.png" mode=""></image></view>
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
				nameData:["安兴","光福","永乐","怀贞","崇业","靖善","靖安","宣义","永达","兰陵","安善","丰安","道德","开明","大兴","昌明","光兴","保宁","昌乐","安乐","延祚","安义","安德"]
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
	.palaceBox{padding: 10rpx 26rpx 0 26rpx;width: 100%;}
	.palaceBox .cont{position: relative;z-index: 2;}
	.palaceBox .child{width: 124rpx;height: 100rpx;border: 1px solid #000000;display: flex;align-items: center; justify-content: center; margin: 0 26rpx 26rpx 0;background-color: rgba(255, 255, 255, 0.7);}
	.palaceBox .child:nth-of-type(4n-1){width: 160rpx;margin-left: 20rpx;}
	.palaceBox .child:nth-of-type(4n){width: 190rpx;margin-right: 0;}
	.palaceBox .child:hover{background: #D0B487; color: #fff;border: 1px solid #9f8253;}
	
</style>
