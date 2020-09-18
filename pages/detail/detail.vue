<template>
	<view>
		<!-- 轮播 -->
		<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#E6B968">
			<block>
				<swiper-item class="swiper-item" >
					<image src="../../static/images/details-img.png" />
				</swiper-item>
			</block>
		</swiper>
		
		<!-- 详情 -->
		<view class="p-3 flex1">
			<view class="price1 colorR font-40 font-w">6980.00</view>
			<view class="flex4 font-22" @click="shareShow">        
				<image src="../../static/images/details-icon.png" class="wh30 mb-1"></image>
				<view>分享</view>
			</view>
		</view>
		<view class="font-32 font-w color2 mx-3 pb-3 bB-f5">陕西大荔冬枣 金勋特技甜枣 1kg装 单果约16-22g 新鲜水果</view>
		<view class="flex p-3 line-h">
			<view class="color8 pr-5">配送</view>
			<view class="flex1 pr-3">
				<image src="../../static/images/shopping-icon.png" class="wh20 mr-1"></image>
				<view>快递配送</view>
			</view>
		</view>
		<view class="f8-16"></view>
		
		<view>
			<view class="color8 p-3">详情</view>
			<view class="px-3 pb-3 flex flex-wrap">
				<view class="pr-5">商品名称：静谧冬枣500g</view>
				<view class="pr-5">商品毛重：0.54g</view>
				<view class="pr-5">包装：简装</view>
				<view class="pr-5">重量：1000-1999g</view>
				<view class="pr-5">原产地：陕西</view>
			</view>
			<image src="../../static/images/details-img1.png" mode="widthFix"></image>
		</view>
		
		
		<view style="height: 100rpx;"></view>
		<view class="p-fX bottom-0 shadowT flex1 bg-white">
			<view class="flex-1 flex4 font-22 bR-f5 line-h" @click="Router.back({route:{delta:1}})">
				<image src="../../static/images/details-icon1.png" class="fh-icon mb-2"></image>
				<view>返回</view>
			</view>
			<view class="btnBox flex">
				<view class="carBtn w-50 colorf bg-05" @click="addShow">加入购物车</view>
				<view class="carBtn w-50 Mgb"
				@click="Router.redirectTo({route:{path:'/pages/submitOrder/submitOrder'}})">立即购买</view>
			</view>
		</view>
		
		<!-- 加入购物车 -->
		<view class="addCar colorf flex0 p-fXY m-a radius10 font-32" v-show="add_show">
			<view>已加入购物车</view>
			<view @click="addShow" class="p-a m-3 top-0 right-0">
				<image src="../../static/images/details-icon5.png" class="wh24" ></image>
			</view>
		</view>
		
		<!-- 分享弹框 -->
		<view class="bg-mask" v-show="share_show">
			<view class="bg-white p-aX bottom-0 radius10-T px-25 pb-4">
				<view class="font-30 py-3">分享</view>
				<view class="flex2 pb-5 pt-3">
					<image src="../../static/images/details-icon3.png" class="img"></image>
					<image src="../../static/images/details-icon4.png" class="img" @click="posterShow"></image>
				</view>
				<view @click="shareShow" class="p-a m-3 top-0 right-0">
					<image src="../../static/images/details-icon2.png" class="wh24" ></image>
				</view>
			</view>
		</view>
		
		<!-- 海报分享 -->
		<view class="bg-mask bg-mask2" v-show="poster_show">
			<view class="p-r mx-25 poster">
				<image src="../../static/images/details-img4.png" mode="widthFix"></image>
				<view class="p-aX top-0">
					<view class="font-36 font-w pt-4 pb-3 text-c">MAY BEE</view>
					<view class="mx-25 p-2 mb-1 shadow">
						<image src="../../static/images/details-img.png" mode="widthFix"></image>
					</view>
					<view class="flex1 m-3">
						<view class="flex-1 pr-3">
							<view class="avoidOverflow2">陕西大荔冬枣 金勋特技甜枣 1kg装 单果约16-22g 新鲜水果</view>
							<view class="price font-32 pt-1 font-w">129</view>
						</view>
						<image src="../../static/images/details-img3.png" class="wh120"></image>
					</view>
					
					<view class="btn80 Mgb">保存图片</view>
					
					<view @click="posterShow" class="p-a m-3 top-0 right-0">
						<image src="../../static/images/details-icon2.png" class="wh24" ></image>
					</view>
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
				share_show:false,
				poster_show:false,
				add_show:false
			}
		},
		methods: {
			
			shareShow(){
				const self = this;
				self.share_show = !self.share_show;
			},
			
			posterShow(){
				const self = this;
				self.share_show = false;
				self.poster_show = !self.poster_show;
			},
			
			addShow(){
				const self = this;
				self.add_show = !self.add_show;
			}
			
		}
	}
</script>

<style scoped>
.swiper-box,.swiper-item,.swiper-item image{width: 750rpx;height: 750rpx;}
.btnBox{width: 560rpx;}
.w145{width: 145rpx;}

.fh-icon{width: 44rpx;height: 34rpx;}
.img{width: 119rpx;height: 147rpx;}
/* .poster{margin-top: 20%;} */

.bg-mask2{overflow-y: auto;padding: 120rpx 0;}
.poster{padding-bottom: 120rpx;}
.btn80{margin-top: 80rpx;}

.addCar{background-color: rgba(0,0,0,0.5);width: 400rpx;height: 160rpx;}
</style>
