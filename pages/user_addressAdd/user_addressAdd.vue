<template>
	<view>
		
		<pageBj></pageBj>
		
		<view class="pageBox">
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="navigateBack"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">添加地址</view>
			</view>
			<view class="myRowBetween mx-3 bg-white rounded10">
				<view class="item d-flex j-sb a-center">
					<view class="ll">姓名</view>
					<view class="rr fs13">
						<input type="text" placeholder="请写姓名" placeholder-class="placeholder">
					</view>
				</view>	
				<view class="item d-flex j-sb a-center">
					<view class="ll">手机号</view>
					<view class="rr fs13" >
						<input type="number" placeholder="请写手机号" placeholder-class="placeholder">
					</view>
				</view>	
				<view class="item d-flex j-sb a-center">
					<view class="ll">所在地区</view>
					<view class="rr fs13 color9">
						请选择<image class="arrowR" src="../../static/images/the-orderl-icon.png" mode=""></image>
					</view>
				</view>	
				<view class="item d-flex j-sb a-center">
					<view class="ll">详细地址</view>
					<view class="rr fs13" >
						<input type="text" placeholder="请填写" placeholder-class="placeholder">
					</view>
				</view>	
				<view class="item d-flex j-sb a-center">
					<view class="ll">设为默认地址</view>
					<view class="rr">
						<switch style="transform:scale(0.75);" color="#d0b487" />
					</view>
				</view>	
				
			</view>
			<view class="submitbtn pdtb15"  style="margin-top: 200rpx;">
				<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="btnBj"><image src="../../static/images/buttonl-icon.png" mode=""></image></view>
					<view class="btnTit">新增</view>
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
				is_show:false,
				score: '',
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			navigateBack(){
				uni.navigateBack({
				});
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding:30rpx 4%}
</style>
