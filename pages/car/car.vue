<template>
	<view>
		<view class="p-s top-0 p-3 font-26 color8 bB-f5 flex1">
			<view>全部（10）</view>
			<view @click="changeState">{{state?'管理':'完成'}}</view>
		</view>
		
		<view class="flex1 p-3 pb-4 mb-2 w750 bg-white" v-for="v in 2" :key="v">
			<image src="../../static/images/shopping-icon.png" class="wh40"></image>
			<!-- <image src="../../static/images/shopping-icon1.png" class="wh40"></image> -->
			<image src="../../static/images/shopping-img.png" class="wh200 ml-2"></image>
			<view class="flex5 font-30 py-0 flex-1 pl-2 h200">
				<view class="avoidOverflow2">ROYALCOVER_罗卡芙家纺 浪漫婚典 真丝绣花提花九件套 爱的乐章</view>
				<view class="flex1">
					<view class="price1 font-36 font-w">29.9</view>
					<view class="count">
						<view class="countIcon"><image src="../../static/images/shopping-icon3.png" class="countIcon1"></image></view>
						<view class="countNum bg-f7">1</view>
						<view class="countIcon"><image src="../../static/images/shopping-icon2.png" class="countIcon2"></image></view>
					</view>
				</view>
			</view>
		</view>
		
		
		
		
		<view style="height: 230rpx;"></view>
		<view class="bg-white bT-e1 flex1 font-24 p-fX bot110">
			<view class="flex pl-3 pr-2 flex-1">
				<image src="../../static/images/shopping-icon1.png" class="wh36 mr-1"></image>
				<view>全选</view>
			</view>
			<view class="flex" v-if="state">
				<view class="font-22 color9 px-3 pr-3">合计：<text class="font-36 price1 font-w">29.9</text></view>
				<view class="w230 Mgb font-32" 
				@click="Router.navigateTo({route:{path:'/pages/submitOrder/submitOrder'}})">去结算</view>
			</view>
			<view class="btn b-e1 mr-3" v-else>删除</view>
		</view>
		
		
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar3.png" mode=""></image>
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
				state:true
			}
		},
		methods: {
			
			changeState(){
				const self = this;
				self.state = !self.state;
			}
			
		}
	}
</script>

<style scoped>
.h200{height: 200rpx;}
.w750{width: 750rpx;}
.w230{width: 230rpx;line-height: 100rpx;text-align: center;}
.bot110{bottom: 110rpx;height: 100rpx;}
.btn{width: 140rpx;line-height: 60rpx;border-radius: 30rpx;font-size: 28rpx;text-align: center;}
.count{border: none;}
</style>