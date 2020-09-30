<template>
	<view>
		
		<view class="p-s top-0 z100">
			<view class="p-r top flex0 bg-white">
				<image src="../../static/images/my-icon.png" mode="widthFix" class="p-aX top-0"></image>
				<view class="flex4">
					<view class="wh160 shadow flex0 radius-5 p-r z10 user">
						<image :src="userData.headImgUrl?userData.headImgUrl:''" class="wh140 radius-5"></image>
					</view>
					<view class="font-32 line-h pt-2 font-w"  v-if="userData&&userData.nickname!=''">{{userData.nickname?userData.nickname:''}}</view>
					<button v-else open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)" 
					style="border-radius: 20rpx;font-size: 15px;background: #EEC337;height: 50rpx;line-height: 50rpx;color: #fff;padding: 0 20rpx;">点击显示微信昵称头像</button>
				</view>
			</view>
			
			<view class="font-26 flex2 py-5 line-h bg-white">
				<view class="flex4"
				@click="Router.navigateTo({route:{path:'/pages/address/address'}})">
					<image src="../../static/images/my-icon2.png" class="icon1 mb-2"></image>
					<view>地址管理</view>
				</view>
				<view class="flex4"
				@click="Router.navigateTo({route:{path:'/pages/refundOrder/refundOrder'}})">
					<image src="../../static/images/my-icon1.png" class="icon2 mb-2"></image>
					<view>退款订单</view>
				</view>
			</view>
			
			<view class="bg-white mx-25 shadow">
				<view class="flexX list p-s top-0 z10 bg-f2">
					<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
					<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待付款</view>
					<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">待发货</view>
					<view class="li" :class="liCurr==3?'on':''" @click="changeLi(3)">待收货</view>
					<view class="li" :class="liCurr==4?'on':''" @click="changeLi(4)">已完成</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white mx-25 radius10 px-2 mt-2" v-for="(item,index) in mainData" :key="index">
			<view class="font-24 flex1 py-3">
				<image src="../../static/images/my-icon3.png" class="wh28"></image>
				<view class="flex-1 pl-1">{{item.create_time}}</view>
				<view class="colorR" v-if="item.pay_status==0">待付款</view>
				<view class="colorR" v-if="item.transport_status==0&&item.pay_status==1">待发货</view>
				<view class="colorR" v-if="item.transport_status==1&&item.pay_status==1">待收货</view>
				<view class="colorR" v-if="item.transport_status==2&&item.pay_status==1">已完成</view>
			</view>
			<view class="flex1">
				<view class="flexX flex-1" :data-id = "item.id"
				@click="Router.navigateTo({route:{path:'/pages/orderDetail/orderDetail?id='+$event.currentTarget.dataset.id}})">
					<image v-for="(c_item,c_index) in item.child"
			:key="c_index"  
			:src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&
						c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" class="wh160 mr-2 flex-shrink"></image>
				</view>
				<view class="text-r w140">
					<view class="price1 font-34 font-w">{{item.price?item.price:''}}</view>
					<view class="font-24 color9">共{{item.child&&item.child.length?item.child.length:''}}件</view>
				</view>
			</view>
			<view class="d-flex j-end py-4">
				<view class="btn60-M b-e1" v-if="item.pay_status==0" @click="orderUpdate(index,1)">取消订单</view>
				<view class="btn60-M b-e1" v-if="item.pay_status==1&&item.transport_status==0"
				 :data-id = "item.id"
				@click="Router.navigateTo({route:{path:'/pages/applyRefund/applyRefund?id='+$event.currentTarget.dataset.id}})">申请退款</view>
				<view class="btn60-M b-e1"  @click="isShow" v-if="item.pay_status==1&&item.transport_status==2">售后服务</view>
				<view class="btn60-M borderM btn2" v-if="item.pay_status==0" @click="goPay(index)">去付款</view>
				<view class="btn60-M borderM btn2" v-if="item.pay_status==1&&item.transport_status==1" @click="orderUpdate(index,2)">确认收货</view>
			</view>
		</view>
		
		<!-- 售后弹框 -->
		<view class="bg-mask flex0" v-show="is_show">
			<view class="mask p-r flex4">
				<image src="../../static/images/my-icon5.png" class="kf"></image>
				<view class="font-34 pt-5">{{phone}}</view>
				<view @click="isShow" class="p-a m-2 top-0 right-0">
					<image src="../../static/images/details-icon2.png" class="wh24" ></image>
				</view>
			</view>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>购物车</view>
			</view>
			<view class="item on">
				<image src="../../static/images/nabar3-a.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				liCurr:0,
				is_show:false,
				userData:{},
				mainData:[],
				searchItem:{
					level:1,
					type:1,
					order_step:0
				},
				Router:this.$Router,
				Utils:this.$Utils,
				phone:''
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.phone = uni.getStorageSync('user_info').thirdApp.phone;
			self.$Utils.loadAll(['getUserData','getMainData'], self);
		},
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			goPay(index) {
				const self = this;	
				self.pay.wxPay = {
					price: self.mainData[index].price,
				};
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.mainData[index].id
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功','none')
									setTimeout(function() {
										self.getMainData(true)
									}, 1000);
								} else {
									self.$Utils.showToast('支付失败','none')
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('支付成功','none')
							setTimeout(function() {
								self.getMainData(true)
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			orderUpdate(index,type) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					
				};
				if(type==1){
					postData.data.status=-1
				}else{
					postData.data.transport_status=2
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			changeLi(i){
				const self = this;
				if(i!=self.liCurr){
					self.liCurr = i;
					if(self.liCurr==0){
						delete self.searchItem.transport_status
						delete self.searchItem.pay_status 
					}else if(self.liCurr==1){
						delete self.searchItem.transport_status
						self.searchItem.pay_status = 0
					}else if(self.liCurr==2){
						self.searchItem.transport_status=0
						self.searchItem.pay_status = 1
					}else if(self.liCurr==3){
						self.searchItem.transport_status=1
						self.searchItem.pay_status = 1
					}else if(self.liCurr==4){
						self.searchItem.transport_status=2
						self.searchItem.pay_status = 1
					};
					self.getMainData(true)
				}
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				const callback = (user, res) => {
					console.log(res)
					self.getUserData(user);
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			
			getUserData(user) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				if(user){
					postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
					};
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		}
	}
</script>

<style scoped>
.top{padding: 60rpx 0 0;}
.icon1{width: 38rpx;height: 45rpx;}
.icon2{width: 51rpx;height: 43rpx;}
.li{width: 20%;}
.w140{width: 140rpx;}
.price1{color: #222;}
.btn60-M{width: 150rpx;margin: 0 0 0 20rpx;}
.btn2{color: #EEC337;}
.mask{width: 600rpx;height: 400rpx;}
.kf{width: 155rpx;height: 190rpx;}
</style>
