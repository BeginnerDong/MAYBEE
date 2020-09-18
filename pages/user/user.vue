<template>
	<view>
		
		<view class="p-r top flex0 bg-white">
			<image src="../../static/images/my-icon.png" mode="widthFix" class="p-aX top-0"></image>
			<view class="flex4">
				<view class="wh160 shadow flex0 radius-5 p-r z10 user">
					<image src="../../static/images/my-img1.png" class="wh140 radius-5"></image>
				</view>
				<view class="font-32 line-h pt-2 font-w">竹蜻蜓</view>
			</view>
		</view>
		
		<view class="font-26 flex2 py-5 line-h bg-white">
			<view class="flex4">
				<image src="../../static/images/my-icon2.png" class="icon1 mb-2"></image>
				<view>地址管理</view>
			</view>
			<view class="flex4">
				<image src="../../static/images/my-icon1.png" class="icon2 mb-2"></image>
				<view>退款订单</view>
			</view>
		</view>
		
		<view class="bg-white mx-25 shadow">
			<view class="flexX list p-s top-0 z10 bg-f2">
				<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
				<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">时令鲜果</view>
				<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">美妆护肤</view>
				<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">服饰箱包</view>
				<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">食品饮水</view>
			</view>
		</view>
		
		<view class="bg-white mx-25 radius10 px-2 mt-2">
			<view class="font-24 flex1 py-3">
				<image src="../../static/images/my-icon3.png" class="wh28"></image>
				<view class="flex-1 pl-1">2020.07.30 11:00:34</view>
				<view class="colorR">代付款</view>
			</view>
			<view class="flex1"
			@click="Router.navigateTo({route:{path:'/pages/orderDetail/orderDetail'}})">
				<view class="flexX flex-1">
					<image src="../../static/images/my-img1.png" class="wh160 mr-2 flex-shrink"></image>
					<image src="../../static/images/my-img1.png" class="wh160 mr-2 flex-shrink"></image>
					<image src="../../static/images/my-img1.png" class="wh160 mr-2 flex-shrink"></image>
					<image src="../../static/images/my-img1.png" class="wh160 mr-2 flex-shrink"></image>
				</view>
				<view class="text-r w140">
					<view class="price1 font-34 font-w">289.9</view>
					<view class="font-24 color9">共3件</view>
				</view>
			</view>
			<view class="d-flex j-end py-4">
				<!-- <view class="btn60-M b-e1">取消订单</view> -->
				<view class="btn60-M b-e1"
				@click="Router.navigateTo({route:{path:'/pages/applyRefund/applyRefund'}})">申请退款</view>
				<view class="btn60-M b-e1"  @click="isShow">售后服务</view>
				<!-- <view class="btn60-M borderM btn2">去付款</view> -->
				<view class="btn60-M borderM btn2">确认收货</view>
			</view>
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
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				is_show:false
			}
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.liCurr = i;
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			}
			
		}
	}
</script>

<style scoped>
.top{padding: 60rpx 0 0;}
.icon1{width: 38rpx;height: 45rpx;}
.icon2{width: 51rpx;height: 43rpx;}
.li{width: 20%;}
.w140{width: 140rpx;}
.price1{color: #222;}
.btn60-M{width: 150rpx;margin: 0 0 0 20rpx;}
.btn2{color: #EEC337;}
.mask{width: 600rpx;height: 400rpx;}
.kf{width: 155rpx;height: 190rpx;}
</style>
