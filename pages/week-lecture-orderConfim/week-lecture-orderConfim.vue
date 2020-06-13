<template>
	<view>
		<pageBj></pageBj>
		
		
			<view class="page-head d-flex a-center j-center">
				<view class="backBtn" @click="Router.back(1)"><image src="../../static/images/back-icon.png" mode=""></image></view>
				<view class="headBj"><image src="../../static/images/head-img.png" mode=""></image></view>
				<view class="tit">确认订单</view>
			</view>
			<view class="pageBox">
			<view class="bg-white boxShaow overflow-h mb-3 mx-3 rounded10 overflow-h">
				<view style="width: 100%;height: 19rpx;"><image src="../../static/images/zhoumo-img4.png" mode=""></image></view>
				<view class="px-3 py-3">
					
					<view class="d-flex a-center j-sb py-3">
						<view class="editMsg rounded10"><input type="text" v-model="addressData.name" placeholder="预约人姓名" placeholder-class="placeholder" /></view>
						<view class="editMsg rounded10"><input type="number" maxlength="11" v-model="addressData.phone" placeholder="预约人电话" placeholder-class="placeholder" /></view>
					</view>
				</view>
			</view>
			<view class="proRow mt-3 mx-3">
				<view class="item mb-3" v-for="item in mainData">
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
					<view class="d-flex j-sb a-center pt-3 border-top">
						<view>优惠券</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length==0">
							暂无优惠券使用<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length==0" @click="couponShow">
							{{couponData.length}}张优惠券可用<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
						<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length>0" @click="couponShow">
							优惠券抵扣-{{pay.coupon[0].price}}<image class="arrowR ml-1" src="../../static/images/the-orderl-icon.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			
			<view class="px-3 pt-5">
				<view class="font-26 color6">支付方式</view>
				<view class="mt-2 d-flex a-center j-sb" @click="payChange('1')">
					<view class="d-flex a-center">
						<image style="width: 34rpx;height: 34rpx;" src="../../static/images/about-img2.png" mode=""></image>
						<view class="ml-1">余额支付</view>
					</view>
					<view class="seltIcon"><image :src="payCurr==1?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
				</view>
				<view class="mt-3 d-flex a-center j-sb" @click="payChange('2')">
					<view class="d-flex a-center">
						<image style="width: 34rpx;height: 34rpx;" src="../../static/images/yuanbao-icon.png" mode=""></image>
						<view class="ml-1">微信支付</view>
					</view>
					<view class="seltIcon"><image :src="payCurr==2?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></view>
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
				
				<scroll-view scroll-y="true" style="height: 400rpx;">
					<view class="pt-1 font-26">
						<view class="item d-flex j-sb a-center px-3 mt-3" v-for="(item,index) in couponData" :key="index" 
						@click="couponChange(index)">
							<view>抵扣券{{item.value}}元</view>
							<view class="seltIcon"><image :src="couponCurr==index?'../../static/images/the-orderl-icon4.png':'../../static/images/the-orderl-icon5.png'" mode=""></image></view>
						</view>
					</view>
				</scroll-view>
				
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
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				Utils:this.$Utils,
				is_couponShow:false,
				couponData:[],
				couponCurr:-1,
				addressData:{
					name:'',
					phone:''
				},
				totalPrice:0,
				pay:{
					coupon:[]
				},
				mainData:[],
				chooseCoupon:[],
				payCurr:2
			}
		},
		
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.$Utils.loadAll(['getUserCouponData'], self);
			self.countTotalPrice()
		},
		
		onShow() {
			const self = this;
			
		},
		
		methods: {
			
			payChange(payCurr){
				const self = this;
				if(payCurr!=self.payCurr){
					self.payCurr = payCurr;
					if(self.payCurr==1){
						delete self.pay.wxPay
						self.pay.balance = {
							price: self.totalPrice,
						};	
					}else if(self.payCurr==2){
						delete self.pay.balance
						self.pay.wxPay = {
							price: self.totalPrice,
						};	
					};
				}
			},
			
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
				/* if(self.pay.coupon.length>=1){
					self.pay.coupon = []
					self.chooseCoupon = []
				}; */
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					/* self.pay.coupon.splice(findItem[0], 1);\
					
					self.chooseCoupon = []; */
						console.log('self.pay',self.pay)
					self.is_show = !self.is_show;
					self.is_couponShow = !self.is_couponShow;
					self.countTotalPrice();
					console.log('self.pay1',self.pay)
					return
				} else {
					/* console.log('self.data.price - self.data.couponTotalPrice',self.totalPrice - self.couponTotalPrice);
					console.log('findCoupon.condition',findCoupon.condition);
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					 console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					}; */
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
					console.log('couponPrice', couponPrice)
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					console.log('self.pay',self.pay)
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
				};
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
				self.countTotalPrice();
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
				
					
				if(self.addressData.name==''||self.addressData.phone==''){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请补全下单信息','none')
					return
				};
				if (self.addressData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.addressData.phone)) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					return;
				}
				var data = {
					
				}
				var orderList = [
					{product_id:self.mainData[0].product_id,count:self.mainData[0].count,type:2,data:data,snap_address:self.addressData}
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
				postData.type = 2;
				postData.snap_address = self.addressData;
				postData.tokenFuncName = 'getProjectToken';
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
				if(self.totalPrice&&self.totalPrice>0){
					postData.payAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type:3,
								thirdapp_id:2,
								user_no:uni.getStorageSync('user_info').user_no,
								account:1,
								count:self.totalPrice,
								trade_info:'购物返积分'
							},
						},
					];
				}
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
										self.$Router.redirectTo({route:{path:'/pages/user-CourseOrder/user-CourseOrder'}})
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
								self.$Router.redirectTo({route:{path:'/pages/user-CourseOrder/user-CourseOrder'}})
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
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	
	.xqbotomBar .payBtn{width: 350rpx;}
	.couponShow{width: 100%;position: fixed;left: 0;right: 0;bottom: 0;height:500rpx;z-index: 50;padding-bottom: 80rpx;}
	.seltIcon{width: 36rpx;height: 36rpx;}
	
	.editMsg{width: 45%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
</style>
