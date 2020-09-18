<template>
	<view>
		<!-- banner -->
		<view class="banner">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#E6B968">
				<block>
					<swiper-item class="swiper-item" >
						<image src="../../static/images/home-banner.png" mode="widthFix" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		
		<!-- banner -->
		<!-- <container>
			<ls-swiper :list="sliderData" imgKey="imgUrl" :loop="true" :dots='true' :autoplay='true' height='125' imgRadius='10' imgWidth='355'/>
		</container> -->
		
		<!-- 金刚区 -->
		<view class="box font-24 color8 line-h py-3 p-r">
			<swiper class="swiper-box" @change="this.currents">
				<block>
					<swiper-item class="swiper-item">
						<view class="flex1">
							<view class="flex4 boxItem"
							@click="Router.redirectTo({route:{path:'/pages/classify/classify'}})">
								<image src="../../static/images/home-icon.png" class="mb-2"></image>
								<view>时令鲜果</view>
							</view>
							<view class="flex4 boxItem"
							@click="Router.redirectTo({route:{path:'/pages/classify/classify'}})">
								<image src="../../static/images/home-icon2.png" class="mb-2"></image>
								<view>美妆护肤</view>
							</view>
							<view class="flex4 boxItem">
								<image src="../../static/images/home-icon3.png" class="mb-2"></image>
								<view>服饰箱包</view>
							</view>
							<view class="flex4 boxItem">
								<image src="../../static/images/home-icon4.png" class="mb-2"></image>
								<view>食品饮料</view>
							</view>
							<view class="flex4 boxItem">
								<image src="../../static/images/home-icon5.png" class="mb-2"></image>
								<view>母婴用品</view>
							</view>
						</view>
					</swiper-item>
				</block>
				<block>
					<swiper-item class="swiper-item" >
						<image src="../../static/images/home-banner.png" mode="widthFix" />
					</swiper-item>
				</block>
			</swiper>
			<view class="boxs flex0">
				<view class="pointBox flex0">
					<view class="point" 
					v-for="(item,index) in 2" :key="index" :class="current==index?'on':''"></view>
				</view>
			</view>
		</view>
		
		<!-- 福利图 -->
		<image src="../../static/images/home-img.png" mode="widthFix" class=" my-3"></image>
		
		<!-- 商品列表 -->
		<view class="mx-25 radius10 bg-white line-h mb-2"
		@click="Router.redirectTo({route:{path:'/pages/detail/detail'}})">
			<image src="../../static/images/home-img1.png" class="img"></image>
			<view class="px-2 py-3">
				<view class="font-32">华美金秋悦月饼礼盒630g</view>
				<view class="price1 font-w font-32 pt-1">59</view>
			</view>
		</view>
		
		<!-- 授权弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask flex4 px-5 pt-5 p-r">
				<image src="../../static/images/logo.png" class="logo mb-4 mt-5"></image>
				<view class="font-34 font-w pt-4">MAY BEE将获取一下公开信息</view>
				<view class="color9 py-4">获取您的公开信息（头像、昵称、地区及性别）</view>
				<view class="flex2 w-100 py-4">
					<view class="btn b-e1" @click="isShow">拒绝</view>
					<view class="btn Mgb" @click="isShow">同意</view>
				</view>
				
				<view @click="isShow" class="p-a m-2 top-0 right-0">
					<image src="../../static/images/icon.png" class="wh50" ></image>
				</view>
			</view>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
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
	import container from '../../components/container/index.vue'
	import LsSwiper from '../../components/ls-swiper/index.vue'
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: true,
				sliderData: ['https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1893203566,3929386202&fm=26&gp=0.jpg'],
				current:0
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			},
			
			currents(e){
				const self = this;
				self.current = e.detail.current;
				console.log(self.current)
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>
<style>page{background-color: #f2f2f2;}</style>
<style scoped>
.banner swiper,.banner swiper-item{width: 700rpx;height: 280rpx;margin: 0 auto;}
.box .boxItem{}
.box .boxItem image{width: 94rpx;height: 94rpx;border-radius: 50%;}
.box swiper,.box swiper-item{width: 670rpx;height: 140rpx;margin: 0 auto;}

.boxs{position: absolute;width: 100%;bottom: 0;}
.pointBox{background-color: #DDE1E4;height: 6rpx;border-radius: 4rpx;}
.point{width: 38rpx;border-radius: 4rpx;height: 6rpx;}
.pointBox .on{background-color: #050404;}

.img{width: 100%;height: 300rpx;}

.logo{width: 315rpx;height: 309rpx;}
.btn{width: 210rpx;line-height: 86rpx;border-radius: 43rpx;font-size: 34rpx;text-align: center;}
</style>