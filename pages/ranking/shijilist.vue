<template>
	<view>
		<view class="list">
			<!-- 0: {score_name: "共同承担家务", score: 5, status: "1", id: 10} -->
			<view class="item" v-for="(item,index) in shiji" :key="index" @tap = "sjdet(item.id)">
				<view class="left">
					<view>{{item.score_name}}</view>
				</view>
				<view class="right">
					<view>+{{item.score}}</view>
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
					{title:'勇救落水儿童',fen:'+5'},
					{title:'捡到手机归还失主',fen:'+15'},
					{title:'消灭二楼会议室火灾',fen:'+25'},
				]
			}
		},
		onLoad(p) {
			let that = this
			uni.setNavigationBarTitle({
			    title: p.name
			});
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/getShijilist',
				method:'GET',
				data:{
					user_id:p.id,
					page:that.page,
					limit:that.limit,
					status:1
				},
				success(res) {
					console.log(res)
					that.shiji = res.data.data
				}
			})
		},
		methods:{
			sjdet(id){
				uni.navigateTo({
					url:'../my/jiludet?id='+id
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
		overflow: hidden;
		text-overflow: ellipsis ;
		white-space: nowrap;
		text-align: left;
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
