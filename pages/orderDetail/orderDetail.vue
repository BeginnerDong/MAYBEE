<template>
	<view>
		<!-- 待支付 -->
		<!-- <view class="p-3 flex1 pb-5">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待支付</view>
				<view>等待买家进行支付~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon.png" class="icon1"></image>
		</view> -->
		<!-- 待发货 -->
		<!-- <view class="p-3 flex1 pb-5">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待发货</view>
				<view>卖家将尽快为您发货，请耐心等待！</view>
			</view>
			<image src="../../static/images/theorderdetails-icon1.png" class="icon2"></image>
		</view> -->
		<!-- 待收货 -->
		<!-- <view class="p-3 flex1 pb-5">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">等待收货</view>
				<view>您的包裹已寄出，请注意查收~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon2.png" class="icon3"></image>
		</view> -->
		<!-- 已收货 -->
		<!-- <view class="p-3 flex1 pb-5">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">买家已收货</view>
				<view>合作愉快哦~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon3.png" class="icon4"></image>
		</view> -->
		<!-- 退款成功-->
		<view class="p-3 flex1 pb-5">
			<view class="color59 line-h flex-1">
				<view class="font-42 pb-3 font-w">退款成功</view>
				<view>已原路返还您的账户，请注意查收~</view>
			</view>
			<image src="../../static/images/theorderdetails-icon4.png" class="icon4"></image>
		</view>
		
		
		<view class="bg-white radius10 overflow-h mb-2 mx-25 px-3 py-4 p-r">
			<view class="pr-5 line-h address">
				<view class="font-30 font-w avoidOverflow">陕西省西安市雁塔区科技路高新大都荟2007</view>
				<view class="font-26 pt-3 color6">张梦瑶 <text class="pl-4">18228933365</text></view>
			</view>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="flex1 pb-3 bB-f5 bg-white p-3 mx-25 mb-2 radius10">
			<image src="../../static/images/my-img1.png" class="wh200"></image>
			<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
				<view class="avoidOverflow2">ROYALCOVER_罗卡芙家纺 浪漫婚典 真丝绣花提花九件套 爱的乐章</view>
				<view class="flex1">
					<view class="price1 font-32 font-w red">29.9</view>
					<view class="font-24 color9">x1</view>
				</view>
			</view>
		</view>
		
		<view class="px-3 font-26 mx-25 radius10 bg-white">
			<view class="flex">
				<view class="color9 pr-4">订单编号：</view>
				<view class="py-3 bB-f5 flex-1">135984568289264</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">下单时间：</view>
				<view class="py-3 bB-f5 flex-1">2020-07-31 14:09:09</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">商品数量：</view>
				<view class="py-3 bB-f5 flex-1">1</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">快递费用：</view>
				<view class="price py-3 bB-f5 flex-1">10</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">商品总额：</view>
				<view class="price py-3 bB-f5 flex-1">2562.26</view>
			</view>
			<view class="flex">
				<view class="color9 pr-4">满减金额：</view>
				<view class="py-3 bB-f5 flex-1">-￥200</view>
			</view>
			<view class="py-3 text-r w-100">
				退款金额： <text class="price1 red font-36 font-w">39.9</text>
			</view>
		</view>
		<view class="p-3 text-r pb-5">
			实付款： <text class="price1 red font-36 font-w">5689.00</text>
		</view>
		
		<!-- 退款成功不显示 -->
		<view class="d-flex j-end p-2 p-fX bottom-0 bg-white shadowT">
			<!-- <view class="btn60-M b-e1">取消订单</view> -->
			<!-- <view class="btn60-M b-e1"
			@click="Router.navigateTo({route:{path:'/pages/applyRefund/applyRefund'}})">申请退款</view> -->
			<view class="btn60-M b-e1"  @click="isShow">售后服务</view>
			<!-- <view class="btn60-M borderM btn2">去付款</view> -->
			<view class="btn60-M borderM btn2">确认收货</view>
		</view>
		
		<!-- 售后弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask p-r flex4">
				<image src="../../static/images/my-icon5.png" class="kf"></image>
				<view class="font-34 pt-5">156825423520</view>
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
				Router:this.$Router,
				is_show:false
			}
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			}
			
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
