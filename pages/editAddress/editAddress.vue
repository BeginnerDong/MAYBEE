<template>
	<view class="line-h">
		
		<view class="px-2">
			
			<view class="bB-f5">
				<view class="font-32 pt-4">姓名</view>
				<input type="text" value="张丹丹" placeholder="请输入" />
			</view>
			<view class="bB-f5">
				<view class="font-32 pt-4">手机号</view>
				<input type="text" value="15268595854" placeholder="请输入" />
			</view>
			<picker mode="region">
				<view class="bB-f5 flex1">
					<view class="flex-1">
						<view class="font-32 pt-4">所在地区</view>
						<view class="py-3">陕西省西安市雁塔区</view>
					</view>
					<image src="../../static/images/address-icon4.png" class="R-icon"></image>
				</view>
			</picker>
			
			<view class="bB-f5">
				<view class="font-32 pt-4">详细地址</view>
				<input type="text" value="高新大都荟" placeholder="请输入" />
			</view>
			<view class="bB-f5 flex1">
				<view class="font-32 py-4">设为默认</view>
				<image src="../../static/images/address-icon2.png" class="mr"></image>
			</view>
			
		</view>
		
		<view class="btn80 Mgb">确定</view>
		<view class="btn80 btn1 bg-05">删除地址</view>
		
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
.btn1{margin: 20rpx;color: #fff;}
</style>
