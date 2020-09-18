<template>
	<view>
		
		<view class="flexX navBox p-s top-0 z10 bg-f2">
			<view class="nav" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
			<view class="nav" :class="navCurr==1?'on':''" @click="changeNav(1)">时令鲜果</view>
			<view class="nav" :class="navCurr==2?'on':''" @click="changeNav(2)">美妆护肤</view>
			<view class="nav" :class="navCurr==3?'on':''" @click="changeNav(3)">服饰箱包</view>
			<view class="nav" :class="navCurr==4?'on':''" @click="changeNav(4)">食品饮水</view>
		</view>
		
		<view class="flex1 flex-wrap line-h mx-25 pb-3"
		@click="Router.redirectTo({route:{path:'/pages/detail/detail'}})">
			<view class="bg-white radius10 overflow-h mt-2 good" v-for="v in 5" :kry="v">
				<image src="../../static/images/classfication-img.png" class="img"></image>
				<view class="p-2">
					<view class="avoidOverflow pb-2">福建红心柚子 红肉香甜蜜柚</view>
					<view class="price1 font-36 font-w">29.9</view>
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
				navCurr:0
			}
		},
		methods: {
			
			changeNav(i){
				const self = this;
				self.navCurr = i;
			}
			
		}
	}
</script>

<style scoped>
.nav{flex-shrink: 0;padding: 30rpx;font-size: 30rpx;line-height: 1;}
.navBox .on{font-weight: bold;position: relative;}
.navBox .on::before{content: '';width: 40rpx;height: 6rpx;border-radius: 3rpx;background-color: #EEC337;bottom: 10rpx;left: 50%;margin-left: -20rpx;position: absolute;}

.good{width: 340rpx;}
.img{width: 340rpx;height: 340rpx;}
</style>
