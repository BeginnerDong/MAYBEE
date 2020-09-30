<template>
	<view>
		
		<view class="flexX navBox p-s top-0 z10 bg-f2">
			<view class="nav" :class="curr==-1?'on':''" @click="change(-1)">全部</view>
			<view class="nav" @click="change(index)"
			:class="curr==index?'on':''" 
			v-for="(item,index) in labelData" :key="index">{{item.title}}</view>
		</view>
		
		<view class="flex1 flex-wrap line-h mx-25 pb-3">
		
			<view  class="bg-white radius10 overflow-h mt-2 good" 
				v-for="(item,index) in mainData" 
				:key="index" 
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})"
			>
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="img"></image>
				<view class="p-2">
					<view class="avoidOverflow pb-2">{{item.title}}</view>
					<view class="price1 font-36 font-w">{{item.price}}</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: [],
				labelData:[],
				curr:-1,
				searchItem:{
					thirdapp_id:2
				},
				idArray:[]
			}
		},
		onLoad(options) {
			const self = this;
			if(options.id){
				self.num = options.id
			};
			console
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
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
			
			change(num){
				const self = this;
				if(num!=self.curr){
					self.curr = num;
					if(self.curr==-1){
						self.searchItem.category_id = ['in',self.idArray]
					}else{
						self.searchItem.category_id = self.labelData[self.curr].id
					};
					self.getMainData(true)
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						};
						if(self.num){
							console.log(123)
							for (var i = 0; i < self.labelData.length; i++) {
								if(self.labelData[i].id==self.num){
									self.curr = i;
									self.searchItem.category_id = self.labelData[i].id
								}
							}
						}else{
							self.searchItem.category_id = ['in',self.idArray]
						}
						self.getMainData()
					}
					
				};
				self.$apis.labelGet(postData, callback);
			},
			
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);	
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.productGet(postData, callback);
			},
			
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
