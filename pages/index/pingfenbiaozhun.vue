<template>
	<view>
		<view class="list">
			<view class="item" v-for="(item,index) in biaozhun" :key="index">
				<view>{{index+1}}、{{item.title}}+{{item.score}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				biaozhun:[
					// {title:'乐于助人',fen:'15'},
					// {title:'拾金不昧',fen:'25'},
					// {title:'尊老爱幼',fen:'35'},
					// {title:'闻达乡里',fen:'45'}
				]
			}
		},
		onLoad() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/scorelist',
				method:'GET',
				success(res) {
					that.biaozhun = res.data.data
				}
			})
		}
	}
</script>

<style>
	.list{
		width: 680rpx;
		margin: 50rpx auto;
	}
	.item view{
		font-size: 30rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
	}
</style>
