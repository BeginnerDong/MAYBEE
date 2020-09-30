<template>
	<view>
		
		<view class="bg-white radius10 overflow-h my-2 mx-25 px-3 py-4 flex1 p-r"
		@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
			<view class="flex-1 pr-5 line-h address"  v-if="addressData.name">
				<view class="font-30 font-w avoidOverflow">{{addressData.city+addressData.detail}}</view>
				<view class="font-26 pt-3 color6">{{addressData.name}} <text class="pl-4">{{addressData.phone}}</text></view>
			</view>
			<view class="flex-1 flex" v-else>
				<image src="../../static/images/theorder-icon.png" class="wh32 mr-1"></image>
				<view class="font-30">添加收货地址</view>
			</view>
			<image src="../../static/images/address-icon4.png" class="R-icon m-2"></image>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="bg-white p-3 mx-25 radius10" v-for="(item,index) in mainData" :key="index">
			<view class="flex1 pb-3 bB-f5 w750 flex-shrink">
				<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" class="wh200"></image>
				<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
					<view class="avoidOverflow2">{{item.product&&item.product.title?item.product.title:''}}</view>
					<view class="price1 font-32 font-w">{{item.product&&item.product.price?item.product.price:''}}</view>
				</view>
			</view>
			<view class="flex1 pt-3">
				<view class="color6">购买数量</view>
				<view class="count">
					<view class="countIcon"  @click="counter(index,'-')"><image src="../../static/images/shopping-icon3.png" class="countIcon1"></image></view>
					<view class="countNum">{{item.count}}</view>
					<view class="countIcon"  @click="counter(index,'+')"><image src="../../static/images/shopping-icon2.png" class="countIcon2"></image></view>
				</view>
			</view>
		</view>
		
		<view class="bg-white px-3 mx-25 mt-2 radius10 line-h flex1 py-4">
			<view class="color6">运费</view>
			<view>￥{{transportPrice}}</view>
		</view>
		
		<view style="height: 120rpx;"></view>
		<view class="bg-white bT-e1 flex1 p-fX bottom-0">
			<view class="font-22 color9 px-3">合计：
				<text class="font-32 price1 font-w">{{realPayPrice}}</text>
			</view>
			<button class="w230 Mgb font-30" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">提交订单</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Utils:this.$Utils,
				Router:this.$Router,
				pay:{
				},
				addressData:{},
				totalPrice:0,
				transportPrice:0,
				realPayPrice:0,
				mainData:[]
			}
		},
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
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
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.transportPrice = 0;
				self.realPayPrice = 0;
				console.log('self.userInfoData',self.userInfoData)
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
					self.transportPrice += self.mainData[i].product.transport_price
				};
				
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				self.transportPrice = parseFloat(self.transportPrice).toFixed(2)
				console.log('self.totalPrice',self.totalPrice)
				console.log('self.transportPrice',self.transportPrice)
				self.realPayPrice = parseFloat(parseFloat(self.totalPrice)+parseFloat(self.transportPrice)).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.realPayPrice > 0) {
					self.pay.wxPay = {
						price: self.realPayPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择收货地址','none')
					return
				}
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({product_id:self.mainData[i].product_id,count:self.mainData[i].count})
				};
				const callback = (user, res) => {
					self.addOrder(orderList,user)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList,user) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					type:1,
					level:1,
					price:self.pay.wxPay.price,
					pay:self.pay,
					snap_address:self.addressData,
					transport_price:self.transportPrice
				};
				postData.type = 1;
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder',postData);
				if(user){
					postData.refreshToken = true
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						var array = self.$Utils.getStorageArray('cartData');
						for (var i = 0; i < orderList.length; i++) {
							for (var j = 0; j < array.length; j++) {
								if(orderList[i].product_id == array[j].id){
									self.$Utils.delStorageArray('cartData', orderList[i], 'id');
								}
							}
						};
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
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功','none')
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user/user'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败','none')
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('支付成功','none')
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user/user'}})
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
			
			counter(index, type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.countTotalPrice();
			},
		}
	}
</script>

<style>
page{background-image: linear-gradient(to bottom, #EEC337, #f5f5f5 40%);background-repeat: no-repeat;}
</style>
<style scoped>
.address{width: 530rpx;}
.h200{height: 200rpx;}
.w230{width: 230rpx;line-height: 100rpx;text-align: center;}
input{width: 300rpx;height: 60rpx;text-align: center;font-size: 24rpx;border-radius: 10rpx;border: 1px solid #e1e1e1;}
</style>
