<template>
	<view>
		
		<!-- banner -->
		<container>
			<ls-swiper :list="sliderData"   :loop="true" :dots='true' :autoplay='true' height='140' imgRadius='10' imgWidth='355' 
			@clickItem="toDetail(index)" @change="change"/>
		</container>
		
		<!-- 金刚区 -->
		<view class="box font-24 color8 line-h py-3 p-r">
			<swiper class="swiper-box" @change="this.currents">
				<block>
					<swiper-item class="swiper-item"  v-for="(item,index) in categoryData" :key="index">
						
						<view class="flex1">
							<view class="flex4 boxItem"  v-for="(c_item,c_index) in item" :key="c_index"
							:data-id = "c_item.id"
							@click="Router.navigateTo({route:{path:'/pages/classify/classify?id='+$event.currentTarget.dataset.id}})">
								<image :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:''" class="mb-2"></image>
								<view>{{c_item.title?c_item.title:''}}</view>
							</view>
						</view>
					</swiper-item>
				</block>
			</swiper>
			
			<view class="boxs flex0">
				<view class="pointBox flex0">
					<view class="point" 
					v-for="(item,index) in categoryData.length" :key="index" :class="current==index?'on':''"></view>
				</view>
			</view>
		</view>
		
		<!-- 福利图 -->
		<image @click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+noticeData.passage1}})" :src="noticeData.mainImg&&noticeData.mainImg[0]?noticeData.mainImg[0].url:''" mode="widthFix" class=" my-3"></image>
		
		<!-- 商品列表 -->
		<view class="mx-25 radius10 bg-white line-h mb-2" 
			v-for="(item,index) in mainData"
			:key="index" 
			:data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})"
		>
			<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:''" class="img"></image>
			<view class="px-2 py-3">
				<view class="font-32">{{item.title}}</view>
				<view class="price1 font-w font-32 pt-1">{{item.price}}</view>
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
					<button class="btn Mgb" @click="isShow" open-type="getUserInfo" @getuserinfo="submit">同意</button>
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
				is_show: false,
				current:0,
				sliderData:[],
				categoryData:[],
				noticeData:{},
				mainData:[]
			}
		},
		components:{
			container,
			LsSwiper
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getCategoryData','getNoticeData','getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.getUserData();
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
				postData.searchItem = {
					thirdapp_id: 2,
					type:1
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			change(e){
				const self = this;
				//console.log(e)
				self.currentId = e.passage1
			},
			
			toDetail(index){
				const self = this;
				self.Router.navigateTo({route:{path:'/pages/detail/detail?id='+self.currentId}})
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 1
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						for (var i = 0; i < res.info.data.length; i++) {
							self.sliderData.push(res.info.data[i])
						}
					}
					console.log('self.sliderData',self.sliderData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getNoticeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id: 8
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.noticeData = res.info.data[0]
					}
					self.$Utils.finishFunc('getNoticeData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getCategoryData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						
						let index = 0;
						let newArray = [];
						while(index < res.info.data.length) {
							newArray.push(res.info.data.slice(index, index += 5));
						}
						self.categoryData = newArray
					}
					console.log('self.categoryData',self.categoryData)
					console.log('self.categoryData',self.categoryData[0])
					self.$Utils.finishFunc('getCategoryData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			submit() {
				const self = this;
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
				postData.noLoading = true;
				if(user){
					postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						if(!uni.getStorageSync('hasTip')){
							uni.setStorageSync('hasTip',true)
							if(self.userData.headImgUrl == ''){
								self.is_show = true
							}else{
								self.is_show = false
							}
						}
					};
				};
				self.$apis.userGet(postData, callback);
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			},
			
			currents(e){
				const self = this;
				self.current = e.detail.current;
				console.log(self.current)
			},
			
			
		}
	};
</script>
<style>page{background-color: #f2f2f2;}</style>
<style scoped>
.banner swiper,.banner swiper-item{width: 700rpx;height: 280rpx;margin: 0 auto;}
.box .boxItem{width: 20%;}
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