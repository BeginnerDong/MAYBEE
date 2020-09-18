<template>
	<view>
		
		<view class="flexX list p-s top-0 z10 bg-f2 shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待退款</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已退款</view>
		</view>
		
		<view class="bg-white mx-25 radius10 px-2 mt-2" v-for="v in 5" :key="v">
			<view class="font-24 flex1 py-3">
				<image src="../../static/images/my-icon3.png" class="wh28"></image>
				<view class="flex-1 pl-1">2020.07.30 11:00:34</view>
				<view class="colorR">待退款</view>
				<view class="colorR">已退款</view>
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
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0
			}
		},
		methods: {
			
			changeLi(i){
				const self = this;
				self.liCurr = i;
			}
			
		}
	}
</script>

<style scoped>
.li{width: 33.33%;padding: 20rpx 0;line-height: 1;}
.list .on::before{bottom: 0;}
.w140{width: 140rpx;}
</style>
