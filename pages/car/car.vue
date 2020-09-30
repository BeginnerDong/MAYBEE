<template>
	<view>
		<view class="p-s top-0 p-3 font-26 color8 bB-f5 flex1">
			<view>全部（{{mainData.length?mainData.length:'0'}}）</view>
			<view v-show="!is_allDelt" @click="allDeltShow">管理</view>
			<view v-show="is_allDelt" @click="allDeltShow">完成</view>
		</view> 
		
		<view class="flex1 p-3 pb-4 mb-2 w750 bg-white" v-for="(item,index) in mainData" :key="index">
			<image  @click="choose(index)" :src="item.isSelect?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1'" class="wh40"></image>
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="wh200 ml-2"></image>
			<view class="flex5 font-30 py-0 flex-1 pl-2 h200">
				<view class="avoidOverflow2">{{item.title}}</view>
				<view class="flex1">
					<view class="price1 font-36 font-w">{{item.price}}</view>
					<view class="count">
						<view class="countIcon" @click="counter(index,'-')"><image src="../../static/images/shopping-icon3.png" class="countIcon1"></image></view>
						<view class="countNum bg-f7">{{item.count}}</view>
						<view class="countIcon" @click="counter(index,'+')"><image src="../../static/images/shopping-icon2.png" class="countIcon2"></image></view>
					</view>
				</view>
			</view>
		</view>
		
		
		
		
		<view style="height: 230rpx;"></view>
		<view class="bg-white bT-e1 flex1 font-24 p-fX bot110">
			<view class="flex pl-3 pr-2 flex-1" @click="chooseAll">
				<image  :src="isChooseAll?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" class="wh36 mr-1"></image>
				<view>全选</view>
			</view>
			<view class="flex" v-if="!is_allDelt">
				<view class="font-22 color9 px-3 pr-3">合计：<text class="font-36 price1 font-w">{{totalPrice}}</text></view>
				<view class="w230 Mgb font-32" 
				 @click="pay">去结算</view>
			</view>
			<view class="btn b-e1 mr-3"  @click="deleteAll()" v-else>删除</view>
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
				count:1,
				mainData:[
					
				],
				is_allDelt:false,
				totalPrice:0,
				isChooseAll:true,
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		onShow() {
			const self = this;
			self.mainData = self.$Utils.getStorageArray('cartData');
			self.checkChooseAll();
			self.countTotalPrice();
		},
		methods: {
			
			pay(e) {
				const self = this;
				const orderList = [
				];
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						orderList.push({
							product_id: self.mainData[i].id,
							count: self.mainData[i].count,
							product: self.mainData[i]
						}, );
					};
				};
				if (orderList.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/submitOrder/submitOrder'
					}
				})
			
			
			},
			
			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
			},
			
			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				self.countTotalPrice();
			},
			
			allDeltShow(){
				const self = this;
				self.is_allDelt = !self.is_allDelt
			},
			
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				self.cartCount = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					self.cartCount += parseInt( self.mainData[i].count)
				};
				self.countTotalPrice();
			},
			
			deleteAll() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认要删除选中商品吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].isSelect){
									self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
								}
							};
							self.mainData = self.$Utils.getStorageArray('cartData');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			},
			
			
			
			choose(index) {
				const self = this;
				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.checkChooseAll();
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				console.log('self.userInfoData',self.userInfoData)
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.totalPrice += self.mainData[i].price * self.mainData[i].count;
					};
				};
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				console.log(self.totalPrice)
			},
			
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