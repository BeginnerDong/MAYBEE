<template>
	<view class="line-h">

		<view class="px-25 py-4 flex1" v-show="id==0" @click="wxAddress">
			<image src="../../static/images/address-icon3.png" class="wx-icon mr-1"></image>
			<view class="flex-1 font-30">获取微信收货地址</view>
			<image src="../../static/images/address-icon4.png" class="R-icon"></image>
		</view>
		<view class="f5Bj-H20" v-show="id==0"></view>

		<view class="px-2">

			<view class="bB-f5">
				<view class="font-32 pt-4">姓名</view>
				<input type="text" v-model="submitData.name" placeholder="请输入" />
			</view>
			<view class="bB-f5">
				<view class="font-32 pt-4">手机号</view>
				<input type="text" maxlength="11" v-model="submitData.phone" placeholder="请输入" />
			</view>
			<picker mode="region" @change="cityChange">
				<view class="bB-f5 flex1">
					<view class="flex-1">
						<view class="font-32 pt-4">所在地区</view>
						<view class="color8 py-3" :style="submitData.city!=''?'color:#222':''">{{submitData.city==''?'请选择':submitData.city}}</view>
					</view>
					<image src="../../static/images/address-icon4.png" class="R-icon"></image>
				</view>
			</picker>

			<view class="bB-f5">
				<view class="font-32 pt-4">详细地址</view>
				<input type="text" v-model="submitData.detail" placeholder="请输入" />
			</view>
			<view class="bB-f5 flex1">
				<view class="font-32 py-4">设为默认</view>
				<image @click="changeDefault" :src="submitData.isdefault==1?'../../static/images/address-icon2.png':'../../static/images/address-icon5.png'"
				 class="mr"></image>
			</view>

		</view>

		<view class="btn80 Mgb" @click="Utils.stopMultiClick(submit)">确定</view>
		<view class="btn80 btn1 bg-05" v-show="id>0" @click="Utils.stopMultiClick(deleteAddress)">删除地址</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				submitData: {
					name: '',
					city: '',
					detail: '',
					phone: '',
					isdefault: 0,
				},
				Utils: this.$Utils,
				id: -1,
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
			if (options.id) {
				self.id = options.id;
				self.$Utils.loadAll(['getMainData'], self);
				uni.setNavigationBarTitle({
					title: '编辑地址'
				})
			} else {
				self.id = 0
			}
		},

		methods: {

			wxAddress() {
				const self = this;
				wx.chooseAddress({
					success: function(res) {
						self.submitData.name = res.userName;
						self.submitData.city = res.provinceName+res.cityName+res.countyName;
						self.submitData.detail = res.detailInfo;
						self.submitData.phone = res.telNumber;
					},
					fail: () => {
						uni.showModal({
							title:'',
							content:'由于您已拒绝该权限，请至小程序设置中手动开启权限'
						})
					}
				})
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
							postData.searchItem.id = self.id;
							postData.tokenFuncName = 'getProjectToken';
							const callback = (res) => {
								if (res) {
									self.$Utils.showToast('操作成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										})
									}, 1000);
								}
							};
							self.$apis.addressDelete(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},

			changeDefault() {
				const self = this;
				if (self.submitData.isdefault == 0) {
					self.submitData.isdefault = 1
				} else {
					self.submitData.isdefault = 0
				}
			},

			cityChange(e) {
				const self = this;
				self.submitData.city = e.detail.value[0] + e.detail.value[1] + e.detail.value[2];
				//self.check();
			},



			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					console.log(res);
					self.submitData.name = res.info.data[0].name;
					self.submitData.city = res.info.data[0].city;
					self.submitData.detail = res.info.data[0].detail;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.isdefault = res.info.data[0].isdefault;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},



			addressUpdate() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('编辑成功', 'success');
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'error')
					};

				};
				self.$apis.addressUpdate(postData, callback);
			},


			addressAdd() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功', 'success');
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg, 'success')
					}

				};
				self.$apis.addressAdd(postData, callback);
			},


			submit() {
				const self = this;
				console.log(34)

				console.log(12)
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.default;
				const pass = self.$Utils.checkComplete(newObject);

				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
						return;
					}
					if (self.id) {

						self.addressUpdate();
					} else {

						self.addressAdd();
					}


				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'success');
				};
			},

		}
	}
</script>
<style>
	page {
		background-color: #fff;
	}
</style>
<style scoped>
	.wx-icon {
		width: 44rpx;
		height: 33rpx;
	}

	input {
		width: 100%;
		line-height: 1;
		padding: 20rpx 0;
		font-size: 28rpx;
	}

	.mr {
		width: 67rpx;
		height: 34rpx;
	}

	.btn80 {
		color: #222;
		margin: 50rpx 20rpx 0;
	}

	.btn1 {
		margin: 20rpx;
		color: #fff;
	}
</style>
