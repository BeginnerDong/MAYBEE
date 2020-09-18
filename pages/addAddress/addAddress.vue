<template>
	<view class="line-h">
		
		<view class="px-25 py-4 flex1">
			<image src="../../static/images/address-icon3.png" class="wx-icon mr-1"></image>
			<view class="flex-1 font-30">获取为您收货地址</view>
			<image src="../../static/images/address-icon4.png" class="R-icon"></image>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2">
			
			<view class="bB-f5">
				<view class="font-32 pt-4">姓名</view>
				<input type="text" value="" placeholder="请输入" />
			</view>
			<view class="bB-f5">
				<view class="font-32 pt-4">手机号</view>
				<input type="text" value="" placeholder="请输入" />
			</view>
			<picker mode="region">
				<view class="bB-f5 flex1">
					<view class="flex-1">
						<view class="font-32 pt-4">所在地区</view>
						<view class="color8 py-3">请输入</view>
					</view>
					<image src="../../static/images/address-icon4.png" class="R-icon"></image>
				</view>
			</picker>
			
			<view class="bB-f5">
				<view class="font-32 pt-4">详细地址</view>
				<input type="text" value="" placeholder="请输入" />
			</view>
			<view class="bB-f5 flex1">
				<view class="font-32 py-4">设为默认</view>
				<image src="../../static/images/address-icon2.png" class="mr"></image>
			</view>
			
		</view>
		
		<view class="btn80 Mgb">确定</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>
<style>page{background-color: #fff;}</style>
<style scoped>
.wx-icon{width: 44rpx;height: 33rpx;}
input{width: 100%;line-height: 1;padding:20rpx 0;font-size: 28rpx;}
.mr{width: 67rpx;height: 34rpx;}
.btn80{color: #222;margin: 50rpx 20rpx 0;}
</style>
