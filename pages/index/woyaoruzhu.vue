<template>
	<view>
		<view class="ruzhu">
			<view class="qiyi" @tap="tourl()">
				<view class="qiyileft">
				<image src="../../static/image/qiye.png"></image>
				<view>入驻企业</view>
				</view>
				<view class="qiyiright">
					<image src="../../static/image/righticon.png"></image>
					</view>
			</view>
			<view class="qiyi" @tap="tishi()">
				<view class="qiyileft">
				<image src="../../static/image/qiye.png"></image>
				<view>入驻村委</view>
				</view>
				<view class="qiyiright">
					<image src="../../static/image/righticon.png"></image>
					</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		methods:{
			tourl(){
				uni.navigateTo({
					url:'./ruzhuqiye'
				})
			},
			tishi(){
				uni.showToast({
					title: '暂未开放',
					icon:'none'
				})
			}
		}
	}
</script>

<style>
	.ruzhu{
		padding-top: 11rpx;
	}
	.qiyi{
		width: 720rpx;
		height: 98rpx;
		background: #FFFFFF;
		border-radius: 14px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 6rpx 0 16rpx;
		box-sizing: border-box;
		margin: 20rpx auto;
	}
	.qiyileft,.qiyiright{
		display: flex;
		align-items: center;
	}
	.qiyileft image{
		width: 66rpx;
		height: 66rpx;
	}
	.qiyileft view{
		font-size: 32rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #333333;
	}
	.qiyiright image{
		width: 66rpx;
		height: 66rpx;
	}
</style>
