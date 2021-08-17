<template>
	<view>
		<view class="list">
			<view class="item" v-for="(item,index) in yizhan" :key="index" @tap="yzdet(item)">
				<view class="box">
					<view class="left">
						<image :src="item.image"></image>
					</view>
					<view class="right">
						<view class="top">
							<view>{{item.title}}</view>
						</view>
						<view class="bottom">
							<view>作者: {{item.author}} / {{item.page}} 页</view>
							<view>{{item.create_time}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				page:1,
				limit:100,
				yizhan:[]
			}
		},
		onLoad() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/yizhan/list',
				method:'GET',
				data:{
					page:this.page,
					limit:this.limit
				},
				success(res) {
					// console.log(res.data.data)
					that.yizhan = res.data.data
				}
			})
		},
		methods:{
			yzdet(item){
				uni.navigateTo({
					url:'./zhanxiangqing?id='+item.id
				})
			}
		}
	}
</script>

<style>
	.bottom{
		display: flex;
		justify-content: space-between;
	}
	.bottom view:nth-child(1){
		font-size: 21rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #666666;
	}
	.bottom view:nth-child(2){
		font-size: 21rpx;
		font-family: Helvetica;
		color: #666666;
	}
	.right{
		width: 525rpx;
		margin-left: 25rpx;
	}
	.top view{
		width: 525rpx;
		white-space:nowrap;
		 overflow:hidden;
		 text-overflow:ellipsis;
		 font-size: 29rpx;
		 font-family: PingFangSC-Regular, PingFang SC;
		 font-weight: 400;
		 color: #323232;
		 margin-top: 10rpx;
		 margin-bottom: 13rpx;
	}
	.box{
		width: 700rpx;
		margin: auto;
		display: flex;
	}
	.left image{
		width: 145rpx;
		height: 96rpx;
	}
	.item{
		border-bottom: 1rpx solid #EEEEEE;
		margin-top: 30rpx;
		padding-bottom: 30rpx;
	}
</style>
