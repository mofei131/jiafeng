<template>
	<view>
		<view class="item" v-for="(item,index) in list" :key="index">
			<view class="name">{{item.name}}</view>
			<view class="ling" v-if="item.status == 1" @tap="ling(item)">未领取</view>
			<view class="wei" v-if="item.status == 2">已领取</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				list:[],
				page:1,
				limit:20
			}
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/goods/myList',
				method:'GET',
				data:{
					user_id:uni.getStorageSync('userInfo').id,
					page:1,
					limit:20
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
			load(){
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/goods/myList',
					method:'GET',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						page:this.page,
						limit:this.limit
					},
					success(res) {
						if(res.data.data.length != 0){
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
			ling(item){
				let that = this
				uni.showModal({
				    title: '提示',
				    content: '是否确认领取',
				    success: function (res) {
				        if (res.confirm) {
				          uni.request({
				          	url:'https://jiafeng.boyaokj.cn/api/goods/use',
				          	method:'GET',
				          	data:{
				          		order_id:item.id
				          	},
				          	success(res) {
				          		if(res.data.code == 200){
				          			uni.showToast({
				          				title:'领取成功',
				          				icon:'none'
				          			})
				          			uni.request({
				          				url:'https://jiafeng.boyaokj.cn/api/goods/myList',
				          				method:'GET',
				          				data:{
				          					user_id:uni.getStorageSync('userInfo').id,
				          					page:that.page,
				          					limit:that.limit
				          				},
				          				success(res) {
				          					that.list = res.data.data
				          				}
				          			})
				          		}else if(res.data.code == -1){
				          			uni.showToast({
				          				title:res.data.message,
				          				icon:'none'
				          			})
				          		}else{
				          			uni.showToast({
				          				title:'系统错误，请联系客服处理',
				          				icon:'none'
				          			})
				          		}
				          	}
				          })
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			}
		}
	}
</script>

<style>
	.ling{
		width: 120rpx;
		height: 48rpx;
		background: #EA433A;
		border-radius: 30rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.wei{
		width: 120rpx;
		height: 48rpx;
		background: #878787;
		border-radius: 30rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.name{
		font-size: 30rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
	}
	.item{
		width: 720rpx;
		height: 96rpx;
		background: #FFFFFF;
		border-radius: 14rpx;
		padding: 0 29rpx 0 27rpx;
		box-sizing: border-box;
		margin: auto;
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-top: 20rpx;
	}
</style>
