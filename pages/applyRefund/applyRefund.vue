<template>
	<view>
		
		<view class="bg-white radius10 overflow-h my-2 mx-25 px-3 py-4 p-r">
			<view class="pr-5 line-h address">
				<view class="font-30 font-w avoidOverflow">陕西省西安市雁塔区科技路高新大都荟2007</view>
				<view class="font-26 pt-3 color6">张梦瑶 <text class="pl-4">18228933365</text></view>
			</view>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="flex1 pb-3 bB-f5 bg-white p-3 mx-25 radius10">
			<image src="../../static/images/my-img1.png" class="wh200"></image>
			<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
				<view class="avoidOverflow2">ROYALCOVER_罗卡芙家纺 浪漫婚典 真丝绣花提花九件套 爱的乐章</view>
				<view class="flex1">
					<view class="price1 font-32 font-w">29.9</view>
					<view class="font-24 color9">x1</view>
				</view>
			</view>
		</view>
		
		<textarea value="" placeholder="请输入申请退款的原因" />
		
		
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

<style>
page{background-image: linear-gradient(to bottom, #EEC337, #f5f5f5 40%);background-repeat: no-repeat;}
</style>
<style scoped>
.address{width: 530rpx;}
.h200{height: 200rpx;}
input{width: 300rpx;height: 60rpx;text-align: center;font-size: 24rpx;border-radius: 10rpx;border: 1px solid #e1e1e1;}
textarea{margin: 20rpx 25rpx;background-color: #fff;padding: 30rpx;border-radius: 10rpx;width: 700rpx;box-sizing: border-box;}
</style>
