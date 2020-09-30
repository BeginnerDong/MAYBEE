<template>
	<view>
		<!-- 待支付 -->
		<view class="p-3 flex1 pb-5" v-if="mainData.pay_status==0&&mainData.order_step==0">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待支付</view>
				<view>等待买家进行支付~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon.png" class="icon1"></image>
		</view>
		<!-- 待发货 -->
		<view class="p-3 flex1 pb-5"  v-if="mainData.pay_status==1&&mainData.transport_status==0&&mainData.order_step==0">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待发货</view>
				<view>卖家将尽快为您发货，请耐心等待！</view>
			</view>
			<image src="../../static/images/theorderdetails-icon1.png" class="icon2"></image>
		</view>
		<!-- 待收货 -->
		<view class="p-3 flex1 pb-5" v-if="mainData.pay_status==1&&mainData.transport_status==1&&mainData.order_step==0">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待收货</view>
				<view>您的包裹已寄出，请注意查收~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon2.png" class="icon3"></image>
		</view>
		<!-- 已收货 -->
		<view class="p-3 flex1 pb-5" v-if="mainData.pay_status==1&&mainData.transport_status==2&&mainData.order_step==0">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">买家已收货</view>
				<view>合作愉快哦~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon3.png" class="icon4"></image>
		</view>
		<!-- 退款成功-->
		<view class="p-3 flex1 pb-5" v-if="mainData.order_step==2">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">退款成功</view>
				<view>已原路返还您的账户，请注意查收~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon4.png" class="icon4"></image>
		</view>
		
		<view class="p-3 flex1 pb-5" v-if="mainData.order_step==1">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">退款中</view>
				<view>退款已申请，等待商家审核~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon4.png" class="icon4"></image>
		</view>
		
		<view class="bg-white radius10 overflow-h mb-2 mx-25 px-3 py-4 p-r">
			<view class="pr-5 line-h address">
				<view class="font-30 font-w avoidOverflow">{{mainData.snap_address.city+mainData.snap_address.detail}}</view>
				<view class="font-26 pt-3 color6">{{mainData.snap_address.name}} <text class="pl-4">{{mainData.snap_address.phone}}</text></view>
			</view>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="flex1 pb-3 bB-f5 bg-white p-3 mx-25 mb-2 radius10" v-for="(item,index) in mainData.child" 
			:key="index">
			<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
						item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" class="wh200"></image>
			<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
				<view class="avoidOverflow2">{{item.product_title}}</view>
				<view class="flex1">
					<view class="price1 font-32 font-w red">{{item.price}}</view>
					<view class="font-24 color9">x{{item.count}}</view>
				</view>
			</view>
		</view>
		
		<view class="px-3 font-26 mx-25 radius10 bg-white">
			<view class="flex">
				<view class="color9 pr-4">订单编号：</view>
				<view class="py-3 bB-f5 flex-1">{{mainData.order_no?mainData.order_no:''}}</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">下单时间：</view>
				<view class="py-3 bB-f5 flex-1">{{mainData.create_time?mainData.create_time:''}}</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">商品数量：</view>
				<view class="py-3 bB-f5 flex-1">{{mainData.child.length?mainData.child.length:''}}</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">快递费用：</view>
				<view class="price py-3 bB-f5 flex-1">{{mainData.transport_price?mainData.transport_price :''}}</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">商品总额：</view>
				<view class="price py-3 bB-f5 flex-1">{{mainData.price?mainData.price:''}}</view>
			</view>
			<view class="py-3 text-r w-100" v-if="mainData.order_step>0">
				退款金额： <text class="price1 red font-36 font-w">{{mainData.price?mainData.price:''}}</text>
			</view>
			<view class="py-3 text-r w-100" v-if="mainData.order_step==0">
				实付款： <text class="price1 red font-36 font-w">{{mainData.price?mainData.price:''}}</text>
			</view>
		</view>
		
		
		<!-- 退款成功不显示 -->
		<view class="d-flex j-end p-2 p-fX bottom-0 bg-white shadowT" v-if="mainData.order_step==0">
			<view class="btn60-M b-e1" @click="orderUpdate(1)" v-if="mainData.pay_status==0">取消订单</view>
			<view class="btn60-M b-e1" v-if="mainData.pay_status==1&&mainData.transport_status==0"
				
				@click="Router.navigateTo({route:{path:'/pages/applyRefund/applyRefund?id='+mainData.id}})">申请退款</view>
			<view class="btn60-M b-e1"  @click="isShow" v-if="mainData.pay_status==1&&mainData.transport_status==2">售后服务</view>
			<view class="btn60-M borderM btn2" v-if="mainData.pay_status==0" @click="goPay">去付款</view>
			<view class="btn60-M borderM btn2"  @click="orderUpdate(2)"  v-if="mainData.pay_status==1&&mainData.transport_status==1">确认收货</view>
		</view>
		
		<!-- 售后弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask p-r flex4">
				<image src="../../static/images/my-icon5.png" class="kf"></image>
				<view class="font-34 pt-5">{{phone}}</view>
				<view @click="isShow" class="p-a m-2 top-0 right-0">
					<image src="../../static/images/details-icon2.png" class="wh24" ></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				is_show:false,
				phone:''
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.phone = uni.getStorageSync('user_info').thirdApp.phone;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			},
			
			goPay() {
				const self = this;	
				self.pay= self.mainData.pay;
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.mainData.id
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
										self.getMainData()
									}, 1000);
								} else {
									self.$Utils.showToast('支付失败','none')
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('支付成功','none')
							setTimeout(function() {
								self.getMainData()
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
			
			orderUpdate(type) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					
				};
				if(type==1){
					postData.data.status=-1
				}else{
					postData.data.transport_status=2
				};
				postData.searchItem = {
					id:self.mainData.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-image: linear-gradient(to bottom, #EEC337, #f5f5f5 40%);background-repeat: no-repeat;}
</style>
<style scoped>
.color59{color: #593F12;}
.icon1{width: 80rpx;height: 78rpx;}
.icon2{width: 74rpx;height: 72rpx;}
.icon3{width: 88rpx;height: 73rpx;}
.icon4{width: 74rpx;height: 74rpx;}

.dz{margin-top: -20rpx;}
.price1,.price{color: #222;}
.red{color: #EA2131;}
.h200{height: 200rpx;}

.btn60-M{width: 150rpx;margin: 0 0 0 20rpx;}
.btn2{color: #EEC337;}
</style>
