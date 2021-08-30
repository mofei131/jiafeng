<template>
	<view>
	<view class="box">
		<!-- <view class="item">
			<view class="title">所属城市：</view>
			 <pick-regions :defaultRegion="defaultRegionCode" @getRegion="handleGetRegion" >
			            <view v-if="city == ''" style="color: #999;width: 420rpx;">请选择城市</view>
									<view style="color: #51565D;width: 420rpx;">{{city}}</view>
			        </pick-regions>
			<image class="cityicon" src="../../static/image/righticon.png"></image>
		</view>
		<view class="item" v-show="city != ''">
			<view class="title">所属企业：</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="name">
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].name}}</text>
					<image src="../../static/image/righticon.png"></image>
				</view>
			</picker>
		</view> -->
		<view class="item">
			<view class="title">所属城市：</view>
			<view class="gather">{{city}}</view>
		</view>
		<view class="item">
			<view class="title">所属企业：</view>
			<view class="gather">{{gongsi}}</view>
		</view>
		<view class="item">
			<view class="title">姓名：</view>
			<view class="gather">{{name}}</view>
		</view>
		<view class="item">
			<view class="title">手机号：</view>
			<view class="gather">{{phone}}</view>
		</view>
		<!-- <view class="item">
			<view class="title">身份证号：</view>
			<view class="gather">{{card}}</view>
		</view> -->
	</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				city:'',
				gongsi:'',
				name:'',
				phone:'',
				// card:''
			}
		},
		onShow() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/ruzhuDetail',
				method:'GET',
				data:{
					user_id:uni.getStorageSync('userInfo').id
				},
				success(res) {
					console.log(res.data)
					that.city = res.data.data.city,
					that.gongsi = res.data.data.gongsi,
					that.name = res.data.data.name,
					that.phone = res.data.data.mobile
					// that.card = res.data.data.idcard
				}
			})
		}
	}
</script>

<style>
	.cityicon{
		width: 66rpx;
		height: 66rpx;
	}
	.btn{
		width: 686rpx;
		height: 70rpx;
		background: #EA433A;
		border-radius: 35rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #FFFFFF;
		display: flex;
		justify-content: center;
		align-items: center;
		margin: auto;
		margin-top: 46rpx;
	}
	.xiangmu{
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
	}
	.gather{
		width: 510rpx;
		margin-left: 30rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #5D6168;
	}
	.title{
		width: 140rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		text-align-last: justify;
	}
	.flex-row{
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.flex-row image{
		width: 66rpx;
		height: 66rpx;
	}
	.box{
		width: 720rpx;
		height: 450rpx;
		background: #FFFFFF;
		border-radius: 14rpx;
		margin: 21rpx auto;
	}
	.item{
		display: flex;
		height: 108rpx;
		width: 680rpx;
		align-items: center;
		justify-content: space-between;
		border-bottom: 1rpx solid #e8e8e8;
		margin: auto;
		position: relative;
	}
	.item:last-child{
		border: 0!important;
	}
</style>
