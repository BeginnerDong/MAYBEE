<template>
	<view>
		
		<view v-for="(item,index) in mainData" :key="index">
			<view class="flex1 py-3 mx-3 bB-f5 address">
				<view class="font-30 pr-5" @click="choose(index)">
					<view>{{item.name}}</view>
					<view class="signM mt-1" v-if="item.isdefault==1">默认</view>
				</view>
				<view class="flex1 flex-1">
					<view class="flex-1">
						<text class="font-28 mb-1">{{item.phone}}</text>
						<view class="avoidOverflow2 color8 w475">{{item.city+item.detail}}</view>
					</view>
					<image src="../../static/images/address-icon.png" class="wh36"
					:data-id = "item.id"
					@click="Router.navigateTo({route:{path:'/pages/addAddress/addAddress?id='+$event.currentTarget.dataset.id}})"></image>
				</view>
			</view>
		</view>
		
		
		
		
		<view class="flex0 py-3 bT-e1 p-fX bottom-0"
		@click="Router.navigateTo({route:{path:'/pages/addAddress/addAddress'}})">
			<image src="../../static/images/address-icon1.png" class="wh26 mr-1"></image>
			<view>新增地址</view>
		</view>
		
	</view>
</template>

<script>
	export default {

		data() {
			return {
				mainData: [],
				Router: this.$Router,
				choosedIndex: -1
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
		},

		onShow(options) {
			const self = this;
			self.getMainData(true)
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

			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta: 1
				})
			},

			getMainData(isNew) {
				const self = this;
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage:1,
						pagesize:10,
						is_page:true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.tokenFuncName = 'getProjectToken';
				postData.order = {
					isdefault:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},





			deleteAddress(id) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认是否删除这个地址',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.searchItem = {};
							postData.searchItem.id = id;
							postData.tokenFuncName = 'getProjectToken';
							const callback = (res) => {
								if (res) {
									self.mainData = [];
									self.getMainData();
								}
							};
							self.$apis.addressDelete(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},


			updateAddress(id) {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.data = {
					isdefault: 1
				}
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressUpdate(postData, callback);
			},

		}
	}
</script>
<style>page{background-color: #fff;}</style>
<style scoped>
.signM{width: 50rpx;line-height: 30rpx;font-size: 20rpx;color: #EEC337;border: 1px solid #EEC337;border-radius: 5rpx;text-align: center;}
.w475{width: 475rpx;}
.address{align-items: flex-start;}
</style>
