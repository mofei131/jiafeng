<template>
	<view>
		<view class="list">
			<view class="item" v-for="(item,index) in shiji" :key="index" @tap="jldet(item)">
				<view class="left">
					<view>{{item.score_name}}</view>
				</view>
				<view class="right">
					<view v-if="item.status == -1">驳回</view>
					<view v-if="item.status == 0">审核中</view>
					<view v-if="item.status == 1">+{{item.score}}</view>
					<image src="../../static/image/righticon.png"></image>
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
				shiji:[
					// {title:'勇救落水儿童',fen:'驳回'},
					// {title:'捡到手机归还失主',fen:'审核中'},
					// {title:'消灭二楼会议室火灾',fen:'25'},
				]
			}
		},
		onLoad(p) {
			// uni.setNavigationBarTitle({
			//     title: p.name
			// });
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/getShijilist',
				method:'GET',
				data:{
					user_id:uni.getStorageSync('userInfo').id,
					// user_id:1,
					page:that.page,
					limit:that.limit,
					status:''
				},
				success(res) {
					console.log(res.data.data)
					that.shiji = res.data.data
				}
			})
		},
		onReachBottom() {
				this.searchChange()
		},
		methods:{
				jldet(item){
					uni.navigateTo({
						url:'./jiludet?id='+item.id
					})
				},
			searchChange(e) {
						let that = this
						that.page++
						uni.request({
							url:'https://layer.boyaokj.cn/api/agreement/myDownload',
							method:'GET',
							data:{
								user_id:uni.getStorageSync('userInfo').user_id,
								page:that.page,
								limit:that.limit,
								status:''
							},
							success(res) {
								for(let i in res.data.data){
									that.shiji.push(res.data.data[i])
								}
							}
						})
					}
		}
	}
</script>

<style>
	.right{
		display: flex;
		align-items: center;
	}
	.right image{
		width: 66rpx;
		height: 66rpx;
	}
	.right view{
		font-size: 36rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #EA433A;
	}
	.left view{
		font-size: 30rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		width: 500rpx;
		 white-space:nowrap;
		   overflow:hidden;
		   text-overflow:ellipsis;
	}
	.item{
		width: 720rpx;
		height: 96rpx;
		background: #FFFFFF;
		border-radius: 14rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-left: 27rpx;
		box-sizing: border-box;
		margin: 20rpx auto;
	}
</style>
