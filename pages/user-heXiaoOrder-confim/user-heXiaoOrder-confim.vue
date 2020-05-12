<template>
	<view>
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">核销订单</view>
			</view>
			
			<view class="proRow mx-3">
				<view class="item mb-3 bg-white">
					<view class="priList" v-for="(item,index) in orderData" :key="index">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待核销</view>
						</view>
						<view class="d-flex a-center j-sb">
							<view class="pic">
								<image src="../../static/images/the-orderl-img1.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow2">Wellfuns 文坊 许巍蓝莲清雅套装礼盒</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="price font-30 font-weight mt-2 d-flex a-center">
										<view class="priceIcon"><image src="../../static/images/about-img2.png" mode=""></image></view>
										<view>666.00</view>
									</view>
									<view class="font-26">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="pb-3 d-flex a-center j-sb font-26 pt-3 border-top">
						<view class="color9">客户信息：</view>
						<view class="d-flex a-center j-end">
							<view>张丹</view>
							<view class="ml-3">15656522121</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn pdtb15"  style="margin-top:100rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/user-heXiaoOrder/user-heXiaoOrder'}})">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">确认</view>
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
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				orderData:1,
				is_hxEwmShow:false
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
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.proRow .item{padding: 0 30rpx;}
	.proRow .item .priList{padding: 30rpx 0;border-top: 1px solid #eee;}
	.proRow .item .priList:first-child{border-top: 0;}	
	.orderNav .tt.on::after{bottom: 0;}
	
</style>
