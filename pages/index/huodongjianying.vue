<template>
	<view>
		<view class="list">
			<view class="item" v-for="(item,index) in jianying" :key="index" @tap = "yzdet(item)">
				<image :src="item.image" mode="widthFix"></image>
				<view>{{item.title}}</view>
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
				jianying:[
					// {title:'夏津市第一次家风活动夏津市第一次家风活动',url:'https://lhcdn.lanhuapp.com/web/imgs/Element61d2bd5d.svg'},
					// {title:'夏津市第二次家风活动夏津市第一次家风活动',url:'https://lhcdn.lanhuapp.com/web/imgs/Element61d2bd5d.svg'},
					// {title:'夏津市第三次家风活动夏津市第一次家风活动',url:'https://lhcdn.lanhuapp.com/web/imgs/Element61d2bd5d.svg'},
				]
			}
		},
		onLoad() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/act/list',
				method:'GET',
				data:{
					page:this.page,
					limit:this.limit
				},
				success(res) {
					console.log(res.data.data)
					that.jianying = res.data.data
				}
			})
		},
		methods:{
			yzdet(item){
				uni.navigateTo({
					url:'./jianyingdet?id='+item.id
				})
			}
		}
	}
</script>

<style>
	.list{
		width: 720rpx;
		margin: auto;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
	}
	.item{
		width: 350rpx;
	}
	.item image{
		width: 350rpx;
		height: 250rpx;
		border-radius: 15rpx;
		margin-top: 30rpx;
	}
	.item view{
		font-size: 32rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #333333;
		white-space:nowrap;
		 overflow:hidden;
		 text-overflow:ellipsis;
		 margin-top: 16rpx;
	}
</style>
