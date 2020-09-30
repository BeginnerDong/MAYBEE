<template>
	<view>
		
		<view class="bg-white radius10 overflow-h my-2 mx-25 px-3 py-4 p-r">
			<view class="pr-5 line-h address">
				<view class="font-30 font-w avoidOverflow">{{mainData.snap_address.city+mainData.snap_address.detail}}</view>
				<view class="font-26 pt-3 color6">{{mainData.snap_address.name}} <text class="pl-4">{{mainData.snap_address.phone}}</text></view>
			</view>
			<image src="../../static/images/theorder-icon1.png" mode="widthFix" class="p-aX bottom-0"></image>
		</view>
		
		<view class="flex1 pb-3 bB-f5 bg-white p-3 mx-25 radius10" v-for="(item,index) in mainData.child" :key="index">
			<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
						item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" class="wh200"></image>
			<view class="flex5 font-30 py-1 flex-1 pl-2 h200">
				<view class="avoidOverflow2">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
				<view class="flex1">
					<view class="price1 font-32 font-w">{{item.price?item.price:''}}</view>
					<view class="font-24 color9">x{{item.price?item.count:''}}</view>
				</view>
			</view>
		</view>
		
		<textarea v-model="passage1" placeholder="请输入申请退款的原因" />
		<view class="btn80 Mgb" style="margin: 50rpx 25rpx;color: #000000;" @click="Utils.stopMultiClick(orderUpdate)">确定</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				passage1:''
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.showLoading();
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.passage1==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入申请退款的原因', 'none');
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					order_step:1,
					passage1:self.passage1
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.Router.redirectTo({route:{path:'/pages/refundOrder/refundOrder'}})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{
						id:self.id,
						user_type:0
					}
				};
				postData.tokenFuncName = 'getProjectToken'
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
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
