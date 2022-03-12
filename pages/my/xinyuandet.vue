<template>
	<view>
		<image class="banner" :src="det.image" mode="widthFix" @tap="clickimg(det.image)"></image>
		<view class="jianlv">
			<view class="mingliang">
				<view class="ming">{{det.name}}</view>
				<view class="fencet">
					<view class="fenxu">许愿</view>
					<image @tap="agreementSuccess(det)" class="fix"
						:src="det.collect==0?'../../static/image/cang2.png':'../../static/image/cang1.png'"></image>
				</view>
			</view>
			<view class="jishou">
				<view class="jihong">{{det.integral}}积分</view>
			</view>
			<view class="yu">剩余:{{det.stock}}</view>
		</view>
		<view class="xiangqing">
			<view class="info">—— 商品信息 ——</view>
			<rich-text class="cont" v-html="content"></rich-text>
		</view>
		<view class="btnbox">
		<view class="btn" @tap="shop()">立即兑换</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				det:'',
				content:'',
				id:''
			}
		},
		onLoad(p) {
			let that = this
			that.id = p.id
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/goods/detail',
				method:'GET',
				data:{
					id:that.id,
					user_id:uni.getStorageSync('userInfo').id
				},
				success(res) {
					that.det = res.data.data
					that.content = that.formatRichText(res.data.data.content)
				}
			})
		},
		methods:{
			agreementSuccess(det) {
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/goods/collect',
					method:'GET',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						goods_id:det.id
					},
					success(res) {
						if(res.data.code == 200){
							if(det.collect == 0){
								uni.showToast({
									title:'许愿成功',
									duration:1000
								})
								uni.request({
									url:'https://jiafeng.boyaokj.cn/api/goods/detail',
									method:'GET',
									data:{
										id:that.id,
										user_id:uni.getStorageSync('userInfo').id
									},
									success(res) {
										that.det = res.data.data
									}
								})
							}else if(det.collect == 1){
								uni.showToast({
									title:'取消成功',
									duration:1000
								})
								uni.request({
									url:'https://jiafeng.boyaokj.cn/api/goods/detail',
									method:'GET',
									data:{
										id:that.id,
										user_id:uni.getStorageSync('userInfo').id
									},
									success(res) {
										that.det = res.data.data
									}
								})
							}
						}else if(res.data.code == -1){
							uni.showToast({
								title:res.message,
								icon:'none'
							})
						}else{
							uni.showToast({
								title:'收藏失败，请联系客服',
								icon:'none'
							})
						}
					}
				})
			},
			shop(){
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/goods/buy',
					method:'GET',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						goods_id:this.det.id
					},
					success(res) {
						if(res.data.code == 200){
							uni.showModal({
									title: '提示',
									content: '兑换成功，请在“我的兑换”里领取',
									showCancel:false,
									success: function (res) {
											// if (res.confirm) {
											// }
									}
							});
							uni.request({
								url:'https://jiafeng.boyaokj.cn/api/goods/detail',
								method:'GET',
								data:{
									id:that.id
								},
								success(res) {
									console.log("运行")
									that.det = res.data.data
									that.content = that.formatRichText(res.data.data.content)
								}
							})
						}else if(res.data.code == -1){
							uni.showToast({
								title:res.data.message,
								icon:'none'
							})
						}else{
							uni.showToast({
								title:'兑换错误，请联系客服',
								icon:'none'
							})
						}
					}
				})
			},
			clickimg(image){
				let arr = []
				arr.push(image)
				uni.previewImage({
					urls:arr,
					indicator:'default',
					longPressActions:true,
				})
			},
			formatRichText(html) {
					let newContent = html.replace(/<img[^>]*>/gi, function(match, capture) {
						match = match.replace(/style="[^"]+"/gi, '').replace(/style='[^']+'/gi, '');
						match = match.replace(/width="[^"]+"/gi, '').replace(/width='[^']+'/gi, '');
						match = match.replace(/height="[^"]+"/gi, '').replace(/height='[^']+'/gi, '');
						return match;
					});
					newContent = newContent.replace(/style="[^"]+"/gi, function(match, capture) {
						match = match.replace(/width:[^;]+;/gi, 'max-width:100%;').replace(/width:[^;]+;/gi, 'max-width:100%;');
						return match;
					});
					newContent = newContent.replace(/<br[^>]*\/>/gi, '');
					newContent = newContent.replace(/\<img/gi,
						'<img style="max-width:100%;height:auto;display:inline-block;margin:10rpx auto;"');
					return newContent;
				}
		}
	}
</script>

<style>
	.mingliang{
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	page{
		padding-bottom: 160rpx;
	}
	.btnbox{
		width: 750rpx;
		height: 160rpx;
		position: fixed;
		bottom: 0;
		left: 0;
		background-color: #fff;
	}
	.btn{
		width: 720rpx;
		height: 76rpx;
		background: #EA433A;
		border-radius: 38rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		margin: auto;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-top: 42rpx;
	}
	.cont{
		width: 710rpx;
		border-radius: 8rpx;
		margin: auto;
	}
	.xiangqing{
		padding-top: 20rpx;
		width: 710rpx;
		margin: auto;
	}
	.info{
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #474747;
		margin-bottom: 10rpx;
		text-align: center;
	}
	.yu{
		font-size: 24rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #666666;
		margin-top: 5rpx;
	}
	.fencet view{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #8C8C8C;
		margin-right: 10rpx;
	}
	.fencet{
		display: flex;
		align-items: center;
	}
	.fencet image{
		width: 36rpx;
		height: 35rpx;
		margin-right: 20rpx;
	}
	.jishou{
		display: flex;
		width: 710rpx;
		margin: auto;
		align-items: center;
		justify-content: space-between;
	}
	.jihong{
		font-size: 48rpx;
		font-family: PingFangSC-Semibold, PingFang SC;
		font-weight: 600;
		color: #FF3D13;
		margin-top: 10rpx;
	}
	.ming{
		font-size: 36rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #2C2B2B;
		width: 590rpx;
		/* white-space:nowrap;
		overflow:hidden;
		text-overflow: ellipsis; */
	}
	.jianlv{
		width: 750rpx;
		margin: auto;
		background-color: #fff;
		padding: 24rpx 20rpx 20rpx 20rpx;
		box-sizing: border-box;
	}
	.banner{
		width: 100%;
	}
</style>
