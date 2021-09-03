<template>
	<view>
		<view class="mingcheng">
			<input type="tetx" v-model="title" placeholder="请输入事迹名称（如：助人为乐、见义勇为）" placeholder-style="color: #6A6A6A;"/>
		</view>
		<!-- <view class="miaoshu">
			<textarea class="mark" type="text" v-model="mark" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #999999;"></textarea>
		</view> -->
		<view class="box">
			<textarea maxlength="200" class="duoh" type="text" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #676767;" v-model="pingjia"></textarea>
			<view class="xianzhi">{{pingjia.length}}/200</view>
			<view class="upload">
				<view class="yshangc" v-for="(item,index) in imgpth" :key="index">
					<image class="yjshang" :src="item"></image>
					<image class="close" src="../../static/image/scclose.png" @tap="close(index)"></image>
				</view>
				<view class="dianji" @tap="upload()" v-if="imgpth.length != 4">
					<image src="../../static/image/scimages.png"></image>
					<view class="zhangshu">{{imgpth.length}}/4</view>
				</view>
			</view>
		</view>
		<view class="xuanze">
			<view class="leixing">请选择事迹类型</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="title">
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].title}}</text>
					<image src="../../static/image/sjrt.png"></image>
				</view>
			</picker>
		</view>
		<view class="jieguo" v-if="false">
			<view>经调查，无此次事件发生</view>
		</view>
		<view class="btn" @tap='submit()'>提交审核</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				title:'',
				mark:'',
				array1: [],
				index1: 0,
				score:'',
				dd:'',
				pingjia:'',
				imgpth:[
					// 'http://dangjian.people.com.cn/NMediaFile/2021/0714/MAIN202107141600240723883355443.jpg',
					// 'http://dangjian.people.com.cn/NMediaFile/2021/0714/MAIN202107141600240723883355443.jpg',
					// 'http://dangjian.people.com.cn/NMediaFile/2021/0714/MAIN202107141600240723883355443.jpg',
				],
				upimage:[],
				id:''
			}
		},
		onLoad() {
			let that = this
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/scorelist',
				method:'GET',
				success(res) {
					console.log(res.data.data)
					that.array1 = res.data.data
				}
			})
		},
		methods:{
			upload(){
				let that = this
				// uni.showModal({
				//     title: '提示',
				//     content: '请选择上传文件类型',
				// 		confirmText:'图片',
				// 		cancelText:'视频',
				// 		cancelColor:'#576B95',
				//     success: function (res) {
				//         if (res.confirm) {
				//             let view = uni.chooseVideo({})
				//         } else if (res.cancel) {
				//             let file = uni.chooseImage({})
				//         }
				//     }
				// });
				
							let file = uni.chooseImage({
								count: 4, //默认9
								sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
								sourceType: ['album','camera'], //从相册选择、摄像头
								success: function(res) {
									// for( let i in res.tempFilePaths){
									// 	that.imgpth.push(res.tempFilePaths[i])
									// }
									that.imgpth = res.tempFilePaths
									console.log(res.tempFilePaths)
								},
							});
						},
			close(index){
				this.imgpth.splice(index,1);
			},
			
			anjianChange1(e) {
				// this.score = this.array1[e.detail.value].id
				this.index1 = e.detail.value;
			},
			submit(){
				console.log("第一步")
				if (!this.title) {
					uni.showToast({
						title: '请填写事迹名称',
						icon: 'none',
					})
					return
				}
				if (!this.pingjia) {
					uni.showToast({
						title: '请填写事迹描述',
						icon: 'none',
					})
					return
				}
				let that = this
				// uni.request({
				// 	url:'https://jiafeng.boyaokj.cn/api/organization/addShiji',
				// 	method:'POST',
				// 	data:{
				// 		user_id:uni.getStorageSync('userInfo').id,
				// 		title:that.title,
				// 		content:that.mark,
				// 		score_id:that.array1[that.index1].id
				// 	},
				// 	success(res) {
				// 		uni.showToast({
				// 			title: '提交审核成功',
				// 			duration:1000
				// 		})
				// 		setTimeout(function() {
				// 		uni.switchTab({
				// 			url:'./index'
				// 		})
				// 		},1000)
				// 	}
				// })
				let an = []
				for(let i in that.imgpth){
					uni.uploadFile({
						url:'https://jiafeng.boyaokj.cn/api/file/upload',
						filePath: that.imgpth[i],
						name: 'file',
						success(res) {
							console.log("第二步")
							an.push(JSON.parse(res.data).data.url)
							that.upimage = an
							if(an.length == that.imgpth.length){
								console.log(JSON.stringify(an))
								uni.request({
									url:'https://jiafeng.boyaokj.cn/api/organization/addShiji',
									method:'POST',
									data:{
										user_id:uni.getStorageSync('userInfo').id,
										title:that.title,
										content:that.pingjia,
										score_id:that.array1[that.index1].id,
										images:JSON.stringify(an)
									},
									success(res) {
										console.log("第三步")
										if(res.data.code == 200){
											uni.showToast({
												title: '提交审核成功',
												duration:1000
											})
											setTimeout(function() {
											uni.switchTab({
												url:'./index'
											})
											},1000)
										}else{
											uni.showToast({
												title: '提交失败，请联系客服',
												duration:1000,
												icon:'none'
											})
										}
									},fail() {
										console.log("错2")
									}
								})
							}
						},fail() {
										console.log("错1")
									}
					})
				}
			}
		}
	}
</script>

<style>
	.xianzhi{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #676767;
		margin-left: 27rpx;
	}
	.yshangc{
		position: relative;
	}
	.close{
		position: absolute;
		width: 34rpx;
		height: 34rpx;
		top: 0;
		right: 0;
	}
	.yjshang{
		width: 161rpx;
		height: 161rpx;
		border-radius: 8rpx;
	}
	.upload{
		display: flex;
		justify-content: space-between;
		width: 654rpx;
		margin: auto;
		padding-top: 20rpx;
	}
	.duoh{
		height: 260rpx;
		width: 680rpx;
		margin: auto;
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #676767;
		padding-top: 8rpx;
		box-sizing: border-box;
	}
	.zhangshu{
		position: absolute;
		font-size: 20rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #979797;
		top: 105rpx;
		left: 68rpx;
	}
	.dianji{
		position: relative;
	}
	.dianji image{
		width: 161rpx;
		height: 161rpx;
	}
	.dd{
		font-size: 26rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #333333;
		margin: 24rpx 0 0 32rpx;
	}
	.box{
		width: 710rpx;
		height: 495rpx;
		background: #F4f4f4;
		border-radius: 14rpx;
		/* border: 1px solid #C9C9C9; */
		margin: 24rpx auto;
		
	}
	.btn{
		width: 686rpx;
		height: 70rpx;
		background: #30AEFF;
		border-radius: 35rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #FFFFFF;
		display: flex;
		justify-content: center;
		align-items: center;
		margin: auto;
		margin-top: 150rpx;
	}
	.jieguo view{
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
	}
	.jieguo{
		width: 710rpx;
		height: 180rpx;
		background: #F4F4F4;
		border-radius: 8rpx;
		margin: auto;
		padding: 18rpx 0 0 25rpx;
		box-sizing: border-box;
	}
	.xiangmu{
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #666666;
	}
	.flex-row image{
		width: 11rpx;
		height: 20rpx;
		margin-left: 13rpx;
	}
	.leixing{
		font-size: 24rpx;
		font-family: PingFangSC-Medium, PingFang SC;
		font-weight: 500;
		color: #6A6A6A;
	}
	page{
		background: #fff;
	}
	.mingcheng input{
		width: 710rpx;
		height: 76rpx;
		background: #F4F4F4;
		border-radius: 16rpx;
		margin: 20rpx auto;
		font-size: 24rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		padding: 0 15rpx 0 15rpx;
		box-sizing: border-box;
	}
	.miaoshu textarea{
		width: 710rpx;
		height: 407rpx;
		background: #F4F4F4;
		border-radius: 8rpx;
		margin: auto;
		padding: 15rpx 25rpx 15rpx 25rpx;
		font-size: 28rpx;
		font-family: PingFangSC-Regular, PingFang SC;
		font-weight: 400;
		color: #51565D;
		box-sizing: border-box;
	}
	.xuanze{
		width: 710rpx;
		height: 76rpx;
		background: #F4F4F4;
		border-radius: 16rpx;
		align-items: center;
		margin: 20rpx auto;
		display: flex;
		justify-content: space-between;
		padding: 0 25rpx 0 15rpx;
		box-sizing: border-box;
	}
</style>
