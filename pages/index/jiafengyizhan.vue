<template>
	<view>
		<!-- <view class="list">
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
		</view> -->
		<view>
			<view class="item" v-for="(item,index) in yizhan" :key="index" @tap="todet(item)">
				<view class="zhu">{{item.name}}</view>
				<view class="shong">
					<view class="cha">{{item.shiji}}</view>
					<view style="color: #EA433A;">+{{item.score}}</view>
					<image src='../../static/image/sjrt.png'></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				yizhan:[],
				id:''
			}
		},
		onLoad(p) {
			console.log(p)
			this.id = p.id
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/shiji_list',
				method:'GET',
				data:{
					org_id:that.id
				},
				success(res) {
					console.log(res.data.data)
					that.yizhan = res.data.data
				}
			})
		},
		methods:{
			todet(item){
				uni.navigateTo({
					url:'../my/jiludet?id='+item.id
				})
			},
			// yzdet(item){
			// 	uni.navigateTo({
			// 		url:'./zhanxiangqing?id='+item.id
			// 	})
			// }
		}
	}
</script>

<style>
	.cha{
		width: 500rpx;
		overflow: hidden;
		text-overflow: ellipsis ;
		white-space: nowrap;
		text-align: right;
	}
	.shong view{
		margin-right: 10rpx;
	}
	.shong{
		display: flex;
		align-items: center;
	}
	.zhu{
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
	/* .item{
		border-bottom: 1rpx solid #EEEEEE;
		margin-top: 30rpx;
		padding-bottom: 30rpx;
	} */
</style>
