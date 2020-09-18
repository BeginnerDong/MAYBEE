<template>
	<view>
		
		<view class="bg-white radius10 overflow-h my-2 mx-25 px-3 py-4 flex1 p-r"
		@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
			<!-- <view class="flex-1 pr-5 line-h address">
				<view class="font-30 font-w avoidOverflow">陕西省西安市雁塔区科技路高新大都荟2007</view>
				<view class="font-26 pt-3 color6">张梦瑶 <text class="pl-4">18228933365</text></view>
			</view> -->
			<view class="flex-1 flex">
				<image src="../../static/images/theorder-icon.png" class="wh32 mr-1"></image>
				<view class="font-30">添加收获地址</view>
			</view>
			<image src="../../static/images/address-icon4.png" class="R-icon m-2"></image>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="bg-white p-3 mx-25 radius10">
			<view class="flex1 pb-3 bB-f5 w750 flex-shrink">
				<image src="../../static/images/my-img1.png" class="wh200"></image>
				<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
					<view class="avoidOverflow2">ROYALCOVER_罗卡芙家纺 浪漫婚典 真丝绣花提花九件套 爱的乐章</view>
					<view class="price1 font-32 font-w">29.9</view>
				</view>
			</view>
			<view class="flex1 pt-3">
				<view class="color6">购买数量</view>
				<view class="count">
					<view class="countIcon"><image src="../../static/images/shopping-icon3.png" class="countIcon1"></image></view>
					<view class="countNum">1</view>
					<view class="countIcon"><image src="../../static/images/shopping-icon2.png" class="countIcon2"></image></view>
				</view>
			</view>
		</view>
		
		<view class="bg-white px-3 mx-25 mt-2 radius10 line-h flex1 py-4">
			<view class="color6">运费</view>
			<view>￥10</view>
		</view>
		
		<view style="height: 120rpx;"></view>
		<view class="bg-white bT-e1 flex1 p-fX bottom-0">
			<view class="font-22 color9 px-3">合计：
				<text class="font-32 price1 font-w">29.9</text>
			</view>
			<view class="w230 Mgb font-30" >提交订单</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
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
