<template>
	<view style="width: 680rpx;margin: auto;padding-top: 40rpx;">
		<view style="font-size: 42rpx;
font-family: PingFangSC-Medium, PingFang SC;
font-weight: 500;
color: #51565D;margin-bottom: 16rpx;">{{title}}</view>
		<view style="font-size: 30rpx;
font-family: PingFangSC-Regular, PingFang SC;
font-weight: 400;
color: #51565D;">{{mark}}</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				title:'',
				mark:'',
				title2:'',
				mark2:'',
				array1: [],
				index1: 0,
				score:'',
				id:'',
				zancun:'',
				zhuangtai:''
			}
		},
		onLoad(p) {
			let that = this
			that.id = p.id,
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/scorelist',
				method:'GET',
				success(res) {
					// console.log(res.data.data)
					that.array1 = res.data.data
				}
			})
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/shijiDetail',
				method:'GET',
				data:{
					id:that.id
				},
				success(res) {
					console.log(res.data.data)
					that.title = res.data.data.title,
					that.mark = res.data.data.content,
					that.index1 = that.array1.findIndex(item => item.id === res.data.data.score_id)
					that.zhuangtai = res.data.data.status
					// console.log()
				}
			})
		},
		onShow() {
			// let that = this
			// console.log(this.title)
		},
		methods:{
			anjianChange1(e) {
				this.score = this.array1[e.detail.value].id
				this.index1 = e.detail.value;
			},
			submit(){
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/organization/addShiji',
					method:'POST',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						score_id:that.score,
						title:that.title,
						content:that.mark
					},
					success(res) {
						uni.showToast({
							title: '重新提交审核成功',
							duration:1000
						})
						setTimeout(function() {
						uni.switchTab({
							url:'./index'
						})
						},1000)
					}
				})
			}
		}
	}
</script>

<style>
	.btn{
		width: 686rpx;
		height: 70rpx;
		background: #EA433A;
		border-radius: 35rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #FFFFFF;
		display: flex;
		justify-content: center;
		align-items: center;
		margin: auto;
		margin-top: 46rpx;
	}
	.jieguo view{
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
	}
	.jieguo{
		width: 710rpx;
		height: 180rpx;
		background: #F4F4F4;
		border-radius: 8rpx;
		margin: auto;
		padding: 18rpx 0 0 25rpx;
		box-sizing: border-box;
	}
	.xiangmu{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #666666;
	}
	.flex-row image{
		width: 11rpx;
		height: 20rpx;
		margin-left: 13rpx;
	}
	.leixing{
		font-size: 24rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #6A6A6A;
	}
	page{
		background: #fff;
	}
	.mingcheng input{
		width: 710rpx;
		height: 76rpx;
		background: #F4F4F4;
		border-radius: 16rpx;
		margin: 20rpx auto;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		padding: 0 15rpx 0 15rpx;
		box-sizing: border-box;
	}
	.miaoshu textarea{
		width: 710rpx;
		height: 407rpx;
		background: #F4F4F4;
		border-radius: 8rpx;
		margin: auto;
		padding: 15rpx 25rpx 15rpx 25rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		box-sizing: border-box;
	}
	.xuanze{
		width: 710rpx;
		height: 76rpx;
		background: #F4F4F4;
		border-radius: 16rpx;
		align-items: center;
		margin: 20rpx auto;
		display: flex;
		justify-content: space-between;
		padding: 0 25rpx 0 15rpx;
		box-sizing: border-box;
	}
</style>
