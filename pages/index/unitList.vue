<template>
	<view>
		<view class="item" v-for="(item,index) in unitlist" :key="index" @tap="tolist(item)">
			<view>{{item.name}}</view>
			<image src='../../static/image/sjrt.png'></image>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				unitlist:[]
			}
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/Organization/getCompany',
				method:'GET',
				success(res) {
					// console.log(res.data.data)
					that.unitlist = res.data.data
				}
			})
		},
		methods:{
			tolist(item){
				uni.navigateTo({
					url:'./yizhanfenlei?id='+item.id
				})
			}
		}
	}
</script>

<style>
	.item view{
		font-size: 30rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #1E2432;
	}
	.item{
		width: 720rpx;
		height: 80rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 20rpx 0 20rpx;
		margin: auto;
		background: #fff;
		box-sizing: border-box;
		margin-top: 16rpx;
		border-radius: 14rpx;
	}
	.item image{
		width: 14rpx;
		height: 28rpx;
	}
</style>
