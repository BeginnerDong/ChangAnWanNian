<template>
	<view>
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">我的</view>
			</view>
			<view class="myHead text-center d-flex a-center flex-column">
				<view class="mny font-weight">20.00</view>
				<view class="d-flex a-center font-26 mt-2"><image style="width:36rpx; height: 36rpx;margin-right: 10rpx;" src="../../static/images/about-img2.png" mode=""></image>余额(元)</view>
			</view>
			
			<view class="px-3 ">
				<view class="font-weight pt-3">充值金额</view>
				<view class="input-money py-2" ><input type="number" value="100" placeholder="请输入金额" /></view>
				
				<view class="specsList d-flex a-center text-center  pt-3" >
					<view class="item position-relative mb-2" :class="curr==index?'on':''" @click="changeCurr(index)" v-for="(item,index) in specsList" :key="index">
						<view class="position-absoluteXY"><image :src="curr==index?'../../static/images/top-upl-icon1.png':'../../static/images/top-upl-icon2.png'" mode=""></image></view>
						<view class="num font-30 color6 font-weight">{{item}}</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top: 80rpx;">
				<button class="btn" type="button" >
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">立即充值</view>
				</button>
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
				wx_info:{},
				is_show:false,
				specsList:['100元','200元','500元','800元','1000元'],
				curr:0,
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack({
				});
			},
			changeCurr(index){
				const self = this;
				self.curr = index;
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	/* @import "../../assets/style/xieyiAlert.css"; */
page{padding-bottom: 60rpx;}	

.myHead{height: 300rpx;}
.myHead .mny{padding-top: 80rpx;font-size: 72rpx;line-height: 72rpx;}
.myRowBetween .item{padding: 20rpx 4%; border-bottom: 1px solid #eee;}

.input-money input{width: 55%;font-size: 30rpx;font-weight: bold;color: #FF2121; height: 60rpx;line-height: 60rpx;border-bottom: 1px solid #999999; display: block;}

.specsList{flex-wrap: wrap;}
.specsList .item{width: 214rpx;height: 101rpx; line-height: 100rpx;margin-right: 24rpx;}
.specsList .item:nth-child(3n){margin-right: 0;}
.specsList .item.on .num{color: #222;}
.specsList .item .num{z-index: 2;position: relative;}

</style>

