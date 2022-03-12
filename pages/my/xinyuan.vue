<template>
	<view>
		<view class="list">
			<view class="item" v-for="(item,index) in list" :key="index">
				<view class="shopimg" @tap="todet(item)">
				<image :src="item.image" mode="aspectFill"></image>
				</view>
				<view class="title" @tap="todet(item)">{{item.name}}</view>
				<view class="tiaojian" @tap="todet(item)">
					<view>{{item.integral}}积分</view>
				</view>
				<view class="sy">
					<view class="xyleft">剩余:{{item.stock}}</view>
					<view class="xyright" @tap="agreementSuccess(item)">
						<view>许愿</view>
						<image  class="fix"
							:src="item.collect==0?'../../static/image/cang2.png':'../../static/image/cang1.png'"></image>
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
				list:[],
				page:1,
				limit:10
			}
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/goods/list',
				method:'GET',
				data:{
					page:1,
					limit:10,
					user_id:uni.getStorageSync('userInfo').id
				},
				success(res) {
					that.list = res.data.data
				}
			})
		},
		onReachBottom() {
			this.page += 1
			this.load()
		},
		methods:{
			agreementSuccess(item) {
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/goods/collect',
					method:'GET',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						goods_id:item.id
					},
					success(res) {
						if(res.data.code == 200){
							if(item.collect == 0){
								uni.showToast({
									title:'许愿成功',
									duration:1000
								})
								uni.request({
									url:'https://jiafeng.boyaokj.cn/api/goods/list',
									method:'GET',
									data:{
										page:1,
										limit:10,
										user_id:uni.getStorageSync('userInfo').id
									},
									success(res) {
										that.list = res.data.data
									}
								})
							}else if(item.collect == 1){
								uni.showToast({
									title:'取消成功',
									duration:1000
								})
								uni.request({
									url:'https://jiafeng.boyaokj.cn/api/goods/list',
									method:'GET',
									data:{
										page:1,
										limit:10,
										user_id:uni.getStorageSync('userInfo').id
									},
									success(res) {
										that.list = res.data.data
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
			load(){
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/goods/list',
					method:'GET',
					data:{
						page:this.page,
						limit:this.limit,
						user_id:uni.getStorageSync('userInfo').id
					},
					success(res) {
						if(res.data.data.length !=0){
							for(let i in res.data.data){
								that.list.push(res.data.data[i])
							}
						}else{
							uni.showToast({
								title:'没有了',
								icon:'none'
							})
						}
					}
				})
			},
			todet(item){
				uni.navigateTo({
					url:'./xinyuandet?id='+item.id
				})
			}
		}
	}
</script>

<style>
	.xyright{
		display: flex;
		align-items: center;
	}
	.xyright view{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #8C8C8C;
		margin-right: 10rpx;
	}
	.xyright image{
		width: 36rpx;
		height: 35rpx;
	}
	.sy{
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 324rpx;
		margin: auto;
		/* height: 70rpx; */
	}
	.shopimg image{
		width: 100%;
	}
	.shopimg{
		width: 348rpx;
		border-radius: 8rpx 8rpx 0rpx 0rpx;
		max-height: 365rpx;
		overflow: hidden;
	}
	.tiaojian view:nth-child(1){
		font-size: 36rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #EA433A;
	}
	.xyleft{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #8C8C8C;
	}
	.list{
		width: 710rpx;
		margin: auto;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
	.item{
		width: 348rpx;
		height: 503rpx;
		background: #FFFFFF;
		box-shadow: 0px 0px 20rpx 0px rgba(202, 188, 188, 0.3);
		border-radius: 8rpx;
		margin-top: 21rpx;
	}
	.title{
		text-overflow: -o-ellipsis-lastline;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 1;
		line-clamp: 1;
		-webkit-box-orient: vertical;
		font-size: 30rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		width: 324rpx;
		margin: auto;
	}
	.tiaojian{
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 324rpx;
		margin: auto;
		/* height: 70rpx; */
	}
</style>
