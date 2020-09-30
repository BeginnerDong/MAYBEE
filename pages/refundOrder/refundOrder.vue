<template>
	<view>
		
		<view class="flexX list p-s top-0 z10 bg-f2 shadow">
			<view class="li" :class="liCurr==0?'on':''" @click="changeLi(0)">全部</view>
			<view class="li" :class="liCurr==1?'on':''" @click="changeLi(1)">待退款</view>
			<view class="li" :class="liCurr==2?'on':''" @click="changeLi(2)">已退款</view>
		</view>
		
		<view class="bg-white mx-25 radius10 px-2 mt-2"v-for="(item,index) in mainData" :key="index" style="padding-bottom: 25rpx;">
			<view class="font-24 flex1 py-3">
				<image src="../../static/images/my-icon3.png" class="wh28"></image>
				<view class="flex-1 pl-1">{{item.create_time}}</view>
				<view class="colorR" v-if="item.order_step==1">待退款</view>
				<view class="colorR" v-if="item.order_step==2">已退款</view>
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
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				liCurr:0,
				mainData:[],
				searchItem:{
					order_step:['>',0]
				}
			}
		},
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
						self.searchItem.order_step = ['in',[1,2]]
					}else if(self.liCurr==1){
						self.searchItem.order_step = 1
					}else if(self.liCurr==2){
						self.searchItem.order_step = 2
					}
					self.getMainData(true)
				}
			},
			
		}
	}
</script>

<style scoped>
.li{width: 33.33%;padding: 20rpx 0;line-height: 1;}
.list .on::before{bottom: 0;}
.w140{width: 140rpx;}
.price1{color: #222;}
</style>
