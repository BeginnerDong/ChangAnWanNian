<template>
	<view>
		<pageBj></pageBj>
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">积分流水</view>
			</view>
			<view class="myExtendTop text-center d-flex a-center flex-column  px-3">
				<view class="money font-weight">126.00</view>
				<view class="fs13 pdt10 d-flex j-center a-center mt-3"><image class="mr-1" style="width: 34rpx;height: 34rpx;" src="../../static/images/racel-icon1.png" mode=""></image>积分(个)</view>
			</view>
			
			<view class="f5Bj-H20">
				<image src="../../static/images/home-icon4.png" mode=""></image>
			</view>
			
			<view class="">
				<view class="myRowBetween mx-3" >
					<view class="item d-flex j-sb a-center border-bottom" v-for="(item,index) in spendData" :key="index">
						<view class="ll">
							<view class="">积分兑换</view>
							<view class="font-26 color6 mt-1">2020.01.17</view>
						</view>
						<view class="rr red">+88</view>
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
				is_show:false,
				spendData:5
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
	@import "../../assets/style/editInfor.css";
	.myExtendTop{padding-top:60rpx;height: 280rpx;}
	.myExtendTop .money{font-size:80rpx; line-height:80rpx;}
	.myRowBetween .item{border-bottom: 1px solid #E1E1E1;}
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
