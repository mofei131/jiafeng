<template>
	<view>
	<view class="box">
		<view class="item">
			<view class="title">所属城市：</view>
			 <pick-regions :defaultRegion="defaultRegionCode" @getRegion="handleGetRegion" >
				 <view style="display: flex; color: #51565D;width: 420rpx;align-items: center;width: 500rpx;justify-content: space-between;">
			            <view v-if="city == ''" >请选择城市</view>
									<view >{{city}} {{area}}</view>
									<image class="cityicon" src="../../static/image/righticon.png"></image>
									</view>
			        </pick-regions>
				<!-- <pickcity @getcity="getCity"></pickcity> -->
			
		</view>
		<view class="item" v-show="city != ''">
			<view class="title">所属企业：</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="name">
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].name}}</text>
					<image src="../../static/image/righticon.png"></image>
				</view>
			</picker>
		</view>
		<view class="item">
			<view class="title">姓名：</view>
			<input class="gather" type="text" v-model="name" placeholder="请输入您的姓名" placeholder-style="color: #999999;" />
		</view>
		<view class="item">
			<view class="title">手机号：</view>
			<input class="gather" type="number" v-model="phone" placeholder="请输入您的手机号" placeholder-style="color: #999999;" />
		</view>
		<view class="item">
			<view class="title">身份证号：</view>
			<input class="gather" type="text" v-model="card" placeholder="请输入您的身份证号" placeholder-style="color: #999999;" />
		</view>
	</view>
	<view class="btn" @tap="ruzhu()" v-if="zhaungtai == -2">申请入驻</view>
	<view class="btn"  v-if="zhaungtai == 0" sabled="disabled">审核中</view>
	<view class="btn" @tap="ruzhu()" v-if="zhaungtai == -1">重新入驻</view>
	</view>
</template>

<script>
	import pickRegions from '@/components/pick-regions/pick-regions.vue'
	export default{
		 components:{ 
			 pickRegions,
			 },
		data(){
			return{
				region:[],
				city:'',
				area:'',
				code:'',
				name:'',
				phone:'',
				card:'',
				array1: [],
				index1: 0,
				org:'',
				zhaungtai:uni.getStorageSync('userInfo').renzheng_status
			}
		},
		computed:{
				// regionName(){
				// 		return this.region.map(item=>item.name).join(' ')
				// }
		},
		onLoad() {
			console.log(uni.getStorageSync('userInfo').renzheng_status == -2)
		},
		methods:{
			 // 获取选择的地区
			handleGetRegion(region){
				let that = this
					this.city = region[1].name
					this.code = region[1].code
					this.area = region[2].name
					console.log(region[1].code)
					uni.request({
						url:'https://jiafeng.boyaokj.cn/api/Organization/getCompany',
						method:'GET',
						data:{
							city_id:region[1].code,
							area_id:region[2].code
							// city_id:370700
						},
						success(res) {
							console.log(res)
							that.array1 = res.data.data
							// that.zhuangtai = res.data.data.status
						}
					})
			},
			anjianChange1(e) {
				// this.org = this.array1[e.detail.value].id
				this.index1 = e.detail.value;
			},
			ruzhu(){
				if (!this.phone) {
					uni.showToast({
						title: '请填写手机号',
						icon: 'none',
					})
					return
				}
				if (!this.name) {
					uni.showToast({
						title: '请填写姓名',
						icon: 'none',
					})
					return
				}
				if (!this.card||this.card.length !=18) {
					uni.showToast({
						title: '请输入正确的身份证号',
						icon: 'none',
					})
					return
				}
				let that = this
				uni.request({
					url:'https://jiafeng.boyaokj.cn/api/organization/ruzhu',
					method:'GET',
					data:{
						user_id:uni.getStorageSync('userInfo').id,
						// org_id:that.org,
						name:that.name,
						mobile:that.phone,
						idcard:that.card,
						org_id:that.array1[that.index1].id
					},
					success(res) {
						// console.log(res)
						uni.showToast({
							title: '提交审核成功',
							duration:1000
						})
						uni.request({
							url:'https://jiafeng.boyaokj.cn/api/wechat/getUserinfo',
							method:'GET',
							data:{
								user_id:uni.getStorageSync('userInfo').user_id
							},
							success(red) {
								// console.log(red.data.data)
								uni.setStorage({
									key:'userInfo',
									data:red.data.data,
								})
							}
						})
						setTimeout(function() {
						uni.switchTab({
							url:'./index'
						})
						},1000)
					}
				})
			}
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
		height: 543rpx;
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
