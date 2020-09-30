<template>
	<view style="background: #fff;">
		<!-- 轮播 -->
		<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="3000" indicator-active-color="#E6B968">
			<block v-for="(item,index) in mainData.bannerImg" :key="index">
				<swiper-item class="swiper-item">
					<image :src="item.url" />
				</swiper-item>
			</block>
		</swiper>

		<!-- 详情 -->
		<view class="p-3 flex1">
			<view class="price1 colorR font-40 font-w">{{mainData.price}}</view>
			<view class="flex4 font-22" style="width: 50px;" @click="shareShow">
				<image src="../../static/images/details-icon.png" class="wh30 mb-1"></image>
				<view>分享</view>
			</view>
		</view>
		<view class="font-32 font-w color2 mx-3 pb-3 bB-f5">{{mainData.title?mainData.title:''}}</view>
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
			<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
			</view>
		</view>


		<view style="height: 100rpx;"></view>
		<view class="p-fX bottom-0 shadowT flex1 bg-white">
			<view class="flex-1 flex4 font-22 bR-f5 line-h" @click="Router.back({route:{delta:1}})">
				<image src="../../static/images/details-icon1.png" class="fh-icon mb-2"></image>
				<view>返回</view>
			</view>
			<view class="btnBox flex">
				<view class="carBtn w-50 colorf bg-05" @click="addCar">加入购物车</view>
				<view class="carBtn w-50 Mgb" @click="goBuy">立即购买</view>
			</view>
		</view>

		<!-- 加入购物车 -->
		<view class="addCar colorf flex0 p-fXY m-a radius10 font-32" v-show="add_show">
			<view>已加入购物车</view>
			<view @click="addShow" class="p-a m-3 top-0 right-0">
				<image src="../../static/images/details-icon5.png" class="wh24"></image>
			</view>
		</view>

		<!-- 分享弹框 -->
		<view class="bg-mask" v-show="share_show">
			<view class="bg-white p-aX bottom-0 radius10-T px-25 pb-4">
				<view class="font-30 py-3">分享</view>
				<view class="flex2 pb-5 pt-3">
					<button open-type="share">
						<image src="../../static/images/details-icon3.png" class="img"></image>
					</button>

					<image src="../../static/images/details-icon4.png" class="img" @click="posterShow"></image>
				</view>
				<view @click="shareShow" class="p-a m-3 top-0 right-0">
					<image src="../../static/images/details-icon2.png" class="wh24"></image>
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
						<image style="width: 614rpx;height: 614rpx;" :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''"></image>
					</view>
					<view class="flex1 m-3">
						<view class="flex-1 pr-3">
							<view class="avoidOverflow2">{{mainData.title}}</view>
							<view class="price font-32 pt-1 font-w">{{mainData.price}}</view>
						</view>
						<image :src="qrUrl" class="wh120"></image>
					</view>



					<view @click="posterShow" style="width: 80rpx;height: 80rpx;display: flex;align-items: center;justify-content: center;"
					 class="p-a  top-0 right-0">
						<image src="../../static/images/details-icon2.png" class="wh24"></image>
					</view>
				</view>

				<view class="btn80 Mgb" @click="submit">保存图片</view>
			</view>
			
		</view>

		<view class="pc-container" style="position: fixed;left:9000px" v-show="canvasShow">
			<!-- <image :src="imgurl" mode="aspectFill" @longpress="saveImage"></image> -->
			<canvas canvas-id="mycanvas"  :style="{width: width+'px',height:height+'px'}"></canvas>

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				share_show: false,
				poster_show: false,
				add_show: false,
				mainData: {},
				canvasShow: true,
				qrUrl: '',
				rpx: 0,
				width:0,
				height:0,
				num:0,
				isCom1:false,
				isCom2:false,
			}
		},

		onLoad(options) {
			const self = this;
			if (options.scene) {
				self.id = decodeURIComponent(options.scene)
			} else {
				self.id = options.id;
			}
			self.$Utils.loadAll(['getMainData', 'getQrCode'], self);

			//获取屏幕宽度，获取自适应单位
			wx.getSystemInfo({
				success: function(res) {
					self.rpx = res.windowWidth / 375;
					self.width = 350* self.rpx 
					self.height = 480*self.rpx
					console.log('self.width',self.width)
				},
			})
		},

		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},

		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: self.mainData.title,
					path: '/pages/detail/detail?id=' + self.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.bannerImg && self.mainData.bannerImg[0] && self.mainData.bannerImg[0].url ?
						self.mainData.bannerImg[0].url : '',
				}
			} else {
				return {
					title: self.mainData.title,
					path: '/pages/detail/detail?id=' + self.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData && self.mainData.bannerImg && self.mainData.bannerImg[0] && self.mainData.bannerImg[0].url ?
						self.mainData.bannerImg[0].url : '',
				}
			}
		},

		methods: {

			writeTextOnCanvas(ctx_2d, lineheight, bytelength, text, startleft, starttop) {
				function getTrueLength(str) { //获取字符串的真实长度（字节长度）
					var len = str.length,
						truelen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							truelen += 2;
						} else {
							truelen += 1;
						}
					}
					return truelen;
				}
			
				function cutString(str, leng) { //按字节长度截取字符串，返回substr截取位置
					var len = str.length,
						tlen = len,
						nlen = 0;
					for (var x = 0; x < len; x++) {
						if (str.charCodeAt(x) > 128) {
							if (nlen + 2 < leng) {
								nlen += 2;
							} else {
								tlen = x;
								break;
							}
						} else {
							if (nlen + 1 < leng) {
								nlen += 1;
							} else {
								tlen = x;
								break;
							}
						}
					}
					return tlen;
				}
				for (var i = 1; getTrueLength(text) > 0; i++) {
					var tl = cutString(text, bytelength);
					ctx_2d.fillStyle = '#222';
					ctx_2d.font = '15px Arial';
					ctx_2d.fillText(text.substr(0, tl).replace(/^\s+|\s+$/, ""), startleft, (i - 1) * lineheight + starttop);
					text = text.substr(tl);
				}
			},

			canvasImage() {
				const self = this;
				self.isCom = false;
				uni.showLoading({
					title: '正在生成图片',
					mask: true
				})
				var myCanvas = uni.createCanvasContext('mycanvas', this);
				//画布尺寸
				//myCanvas.draw()//清空原来的画图内容
				//myCanvas.save();
				// 坐标(0,0) 表示从此处开始绘制，相当于偏移。
				//背景色

				myCanvas.drawImage('../../static/images/details-img4.png', 0, 0, 350*self.rpx, 480*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true)
				myCanvas.textAlign = 'center' //文字居中
				myCanvas.font = 'bold 18px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText('MAY BEE', 175 *self.rpx, 30*self.rpx, 350 *self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				/* if (this.mainData && this.mainData.bannerImg && this.mainData.bannerImg[0]) {
					var bannerImg = this.mainData.bannerImg[0].url;
					wx.downloadFile({
						url: bannerImg, //网络路径
						success: function(res) {
							console.log('bannerImg', res)
							myCanvas.drawImage(res.tempFilePath, 20*self.rpx, 70, 307*self.rpx, 307*self.rpx);
							myCanvas.save()
							myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
							myCanvas.draw(true)
						}
					});
				}; */
				var bannerImg = this.mainData.bannerImg[0].url;
				uni.getImageInfo({
					src: bannerImg, //网络路径
					success: function(res) {
						console.log('bannerImg', res)
						self.isCom1 = true;
						myCanvas.drawImage(res.path, 20*self.rpx, 70*self.rpx, 307*self.rpx, 307*self.rpx);
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
				});
				/* var bannerImg = this.mainData.bannerImg[0].url;
				myCanvas.drawImage(bannerImg, 20*self.rpx, 70, 307*self.rpx, 307*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true) */
			
				//return
				//myCanvas.textAlign = 'start'
				myCanvas.setTextAlign('left')
				/* myCanvas.font = '15px Arial'
				myCanvas.fillStyle = '#222';
				myCanvas.fillText(self.mainData.title, 20*self.rpx, 425); */
				this.writeTextOnCanvas(myCanvas, 20, 35*self.rpx, self.mainData.title, 20*self.rpx, 425*self.rpx)
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				myCanvas.font = 'bold 16px Arial'
				myCanvas.fillStyle = '#EF2732';
				myCanvas.fillText('￥' + self.mainData.price, 20*self.rpx, 470*self.rpx);
				myCanvas.save()
				myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
				myCanvas.draw(true);
				var qr = self.qrUrl;
				uni.getImageInfo({
					src: qr,
					complete(res)  {
						console.log('res',res)
						self.isCom2 = true;
						myCanvas.drawImage(res.path, 275*self.rpx,
							400*self.rpx, 60*self.rpx, 60*self.rpx);
							
						myCanvas.save()
						myCanvas.restore(); //恢复之前保存的绘图上下文 恢复之前保存的绘图上下午即状态 可以继续绘制
						myCanvas.draw(true)
					}
					
				});
				console.log('self.isCom2',self.isCom2)
				console.log('self.isCom1',self.isCom1)
				var interval = setInterval(function() {
					self.num++
					if (self.isCom2&&self.isCom1) {
						clearInterval(interval)
						
						self.canvasToTempFilePath()
					}
				}, 1000);
			},
			
			canvasToTempFilePath(){
				//const self = this;
				uni.canvasToTempFilePath({
					canvasId: 'mycanvas',
					success: (res) => {
						console.log(res)
						this.imgurl = res.tempFilePath;
						this.savePoster()
						uni.hideLoading();
					},
				}, this);
			},

			submit() {
				this.canvasImage()
				
			},


			savePoster() {
				let self = this
				//若二维码未加载完毕，加个动画提高用户体验
				wx.showToast({
					icon: 'loading',
					title: '正在下载图片',
					duration: 1000
				})
				//判断用户是否授权"保存到相册"
				wx.getSetting({
					success(res) {
						//没有权限，发起授权
						if (!res.authSetting['scope.writePhotosAlbum']) {
							wx.authorize({
								scope: 'scope.writePhotosAlbum',
								success() { //用户允许授权，保存图片到相册
									self.savePhoto();
								},
								fail() { //用户点击拒绝授权，跳转到设置页，引导用户授权
									console.log('fail')
									wx.showToast({
										icon: 'none',
										title: '请至小程序设置中开启相册权限',
										duration: 1000
									})
								}
							})
						} else { //用户已授权，保存到相册
							self.savePhoto()
						}
					}
				})
			},

			savePhoto() {
				let self = this
				console.log()
				wx.saveImageToPhotosAlbum({
					filePath: self.imgurl,
					success(res) {
						wx.showToast({
							title: '保存成功',
							icon: "success",
							duration: 1000
						})
						/* var myCanvas = uni.createCanvasContext('mycanvas', this);
						console.log(myCanvas.width)
						myCanvas.clearRect(0, 0, 345, 380);
						myCanvas.save()
						myCanvas.restore() */
					}
				})
			},

			getQrCode() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.id,
					page: 'pages/detail/detail',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.qrUrl = res.info.url;
					}
					self.$Utils.finishFunc('getQrCode');
				};
				self.$apis.getQrCode(postData, callback);
			},

			goBuy() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.orderList.push({
					product_id: self.mainData.id,
					count: 1,
					product: self.mainData
				}, );
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/submitOrder/submitOrder'
					}
				})
				uni.setStorageSync('canClick', true);
			},

			addCar() {
				const self = this;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].id == self.id) {
						var target = array[i]
					}
				}
				if (target) {
					target.count = target.count + 1
				} else {
					var target = self.mainData;
					target.count = 1;
					target.isSelect = true;
				}
				self.addShow();
				self.$Utils.setStorageArray('cartData', target, 'id', 999);
			},

			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						
						
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},

			shareShow() {
				const self = this;
				self.share_show = !self.share_show;
			},

			posterShow() {
				const self = this;
				self.share_show = false;
				self.poster_show = !self.poster_show;
			},

			addShow() {
				const self = this;
				self.add_show = !self.add_show;
			}

		}
	}
</script>

<style scoped>
	.swiper-box,
	.swiper-item,
	.swiper-item image {
		width: 750rpx;
		height: 750rpx;
	}

	.btnBox {
		width: 560rpx;
	}

	.w145 {
		width: 145rpx;
	}

	.fh-icon {
		width: 44rpx;
		height: 34rpx;
	}

	.img {
		width: 119rpx;
		height: 147rpx;
	}

	/* .poster{margin-top: 20%;} */

	.bg-mask2 {
		overflow-y: auto;
		padding: 120rpx 0;
	}

	.poster {
		padding-bottom: 120rpx;
	}

	.btn80 {
		margin-top: 80rpx;
	}

	.addCar {
		background-color: rgba(0, 0, 0, 0.5);
		width: 400rpx;
		height: 160rpx;
	}

	page {
		background: #fff !important;
	}
</style>
