<template>
	<view style="height: 100%;">
		<pageBj></pageBj>
		
		<view class="Box">
		
			<view class="page-head d-flex a-center j-center" :style="{marginTop:statusBar + 'px'}">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">确认订单</view>
			</view>
		<view class="pageBox pb-4" :style="{marginTop:44+statusBar + 'px'}">
			<view class="bg-white boxShaow overflow-h mb-3 mx-3 rounded10 overflow-h">
				<view style="width: 100%;height: 19rpx;"><image src="../../static/images/zhoumo-img4.png" mode=""></image></view>
				<view class="px-3 py-3">
					
					<view class="d-flex j-sb a-center" v-if="addressData.name"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28">
							<view class="d-flex mt-1">
								<view class="mr-3 color2">{{addressData.name}}</view>
								<view class="color6">{{addressData.phone}}</view>
							</view>
							<view class="mt-2">{{addressData.city+addressData.detail}}</view>
						</view>
						<view class="d-flex j-end" style="width: 20%;">
							<image class="arrowR" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
					</view>
					
					<view class="d-flex j-center py-3" v-else @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28 main-text-color d-flex a-center ">
							<view style="width: 32rpx;height:32rpx;margin-right: 20rpx;"><image src="../../static/images/addressl-icon2.png" mode=""></image></view>
							<view class="font-30">添加收货地址</view>
						</view>
					</view>
				</view>
			</view>
			<view class="proRow mt-3 mx-3">
				<view class="item mb-3" v-for="item in mainData" :key="item.id">
					<view class="d-flex j-sb mb-3">
						<view class="pic"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
						<view class="infor">
							<view class="tit avoidOverflow">{{item.product&&item.product.title?item.product.title:''}}</view>
							<!-- <view class="d-flex font-24 color6 mt-1">
								<view class="specsBtn mr-1">精装品5斤</view>
							</view> -->
							<view class="d-flex j-sb B-price">
								<view class="price font-30 font-weight mt-2 d-flex a-center">
									<view class="priceIcon"><image src="../../static/images/about-img2.png" mode=""></image></view>
									<view>{{item.product&&item.product.title?item.product.price:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="d-flex j-sb a-center py-3 border-top">
						<view>购买数量</view>
						<view class="d-flex j-end a-center color6 font-26">
							<view class="numBox d-flex a-center">
								<view class="btn" @click="counter('-')">-</view>
								<view class="num">{{item.count}}</view>
								<view class="btn pubBj white add" @click="counter('+')">+</view>
							</view>
						</view>
					</view>
					
					<view class="d-flex j-sb a-center py-3 border-top">
						<view>优惠券</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length==0" @click="couponShow">
							暂无优惠券使用<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length==0" @click="couponShow">
							{{couponData.length}}张优惠券可用<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length>0" @click="couponShow">
							优惠券抵扣-{{pay.coupon[0].price}}<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
					</view>
					
					<view class="d-flex j-sb a-center pt-3 border-top">
						<view>备注</view>
						<view class="d-flex  a-center color6 font-26">
							<input placeholder="请填写备注(选填)" style="text-align: right;" v-model="passage1"/>
						</view>
					</view>
				</view>
			</view>
			
			
			
			
			<view class="xqbotomBar pl-3" style="height: 100rpx;">
				<view class="d-flex a-center mr-3 font-26">
					<view>总计</view>
					<view class="price font-30 font-weight ml-1 d-flex a-center">
						<view class="priceIcon"><image src="../../static/images/about-img2.png" mode=""></image></view>
						<view>{{totalPrice}}</view>
					</view>
				</view>
				
				<button class="payBtn d-flex j-center a-center" style="width: 300rpx;height: 100rpx;margin: 0;border-radius: 0;"
				open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">
					<view class="payBtnBj"><image src="../../static/images/the-orderl-icon3.png" mode=""></image></view>
					<view class="position-relative main-text-color font-30 " style="z-index: 2;">立即支付</view>
				</button>
			</view>
			
			<!-- 优惠券 -->
			<view class="black-bj" v-show="is_show"></view>
			<view class="couponShow whiteBj radius10" v-show="is_couponShow">
				<view class="d-flex j-sb px-3 py-3 border-bottom">
					<view class="font-26 color9" @click="couponShow">取消</view>
					<view class="">优惠券</view>
					<view class="font-26 main-text-color"  @click="useCoupon">确定</view>
				</view>
				
				
				<view class="pt-1 font-26">
					<view class="item d-flex j-sb a-center px-3 mt-3" v-for="(item,index) in couponData" :key="index" 
					@click="couponChange(index)">
						<view>抵扣券{{item.value}}元</view>
						<view class="seltIcon"><image :src="couponCurr==index?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
					</view>
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
				is_show:false,
				curr:1,
				count:1,
				is_couponShow:false,
				couponData:[],
				couponCurr:-1,
				addressData:{
					
				},
				totalPrice:0,
				pay:{
					coupon:[]
				},
				mainData:[],
				Utils:this.$Utils,
				chooseCoupon:[],
				statusBar:app.globalData.statusBar,
				passage1:''
			}
		},
		
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			uni.removeStorageSync('choosedAddressData');
			
			self.$Utils.loadAll(['getUserCouponData'], self);
			self.countTotalPrice()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
				
			}else{
				self.getAddressData()
			};
		},
		
		methods: {
			
			getUserCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					pay_status:1,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					self.$Utils.finishFunc('getUserCouponData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			useCoupon() {
				const self = this;	
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
			/* 	console.log(index)
				console.log(self.couponData);
				console.log(self.couponData[0]); */
				
				if(self.couponData[self.couponCurr]&&self.couponData[self.couponCurr].id){
					var id = self.couponData[self.couponCurr].id;
				}else{
					self.pay.coupon = []
					self.chooseCoupon = [];
					self.is_show = !self.is_show;
					self.is_couponShow = !self.is_couponShow;
					self.countTotalPrice();
					console.log('self.pay',self.pay)
					return
				}
				
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.showCoupon = false;
				
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.is_show = !self.is_show;
					self.is_couponShow = !self.is_couponShow;
					self.countTotalPrice();
					console.log('self.pay',self.pay)
					return
				} else {
					
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					/* if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					}; */
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
				};
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
				self.countTotalPrice();
			},
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}else{
						self.addressData = {}
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
				};
				self.totalPrice = (parseFloat(self.totalPrice) - parseFloat(self.couponTotalPrice)).toFixed(2)
				//console.log('score',score)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.mainData[0].count++;
				} else {
					if (self.mainData[0].count> 1) {
						self.mainData[0].count--;
					}
				};			
				self.countTotalPrice();
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择收货地址','none')
					return
				};
				var data = {
					passage1:self.passage1
				}
				var orderList = [
					{product_id:self.mainData[0].product_id,count:self.mainData[0].count,type:1,data:data,snap_address:self.addressData}
				];
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.type = 1;
				postData.snap_address = self.addressData;
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					passage1:self.passage1
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				if(self.pay.wxPay&&self.pay.wxPay.price>0){
					postData.payAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type:3,
								thirdapp_id:2,
								user_no:uni.getStorageSync('user_info').user_no,
								account:1,
								count:self.pay.wxPay.price,
								trade_info:'购物返积分'
							},
						},
					];
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								/* self.is_show = true
								self.is_payVipShow = !self.is_payVipShow;
								self.is_PayShow = false; */
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			navigateBack(){
				uni.navigateBack({
				});
			},
			
			couponChange(index){
				const self = this;
				if(self.couponCurr!=index){
					self.couponCurr = index;
				}else{
					self.couponCurr = -1
				}
			},
			
			couponShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;box-sizing: border-box;}
	
	
	.xqbotomBar .payBtn{width: 350rpx;}
	.couponShow{width: 100%;position: fixed;left: 0;right: 0;bottom: 0;height:500rpx;z-index: 50;padding-bottom: 80rpx;}
	.seltIcon{width: 36rpx;height: 36rpx;}
</style>
