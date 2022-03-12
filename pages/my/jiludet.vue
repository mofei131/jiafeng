<template>
	<view>
		<view class="mingcheng" v-if="zhuangtai == 0">
			<input type="tetx" v-model="title" disabled='zhuangtai == 0' placeholder="请输入事迹名称（如：助人为乐、见义勇为）" placeholder-style="color: #6A6A6A;"/>
		</view>
		<!-- <view class="miaoshu" v-if="zhuangtai == 0">
			<textarea class="mark" type="text" disabled='zhuangtai == 0' v-model="mark" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #999999;"></textarea>
		</view> -->
		<view class="box" v-if="zhuangtai == 0">
			<textarea maxlength="200" disabled='zhuangtai == 0' class="duoh" type="text" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #676767;" v-model="pingjia"></textarea>
			<view class="xianzhi">{{pingjia.length}}/200</view>
			<view class="upload">
				<view class="yshangc" v-for="(item,index) in imgpth" :key="index">
					<image class="yjshang" :src="item" @tap="clickimg()"></image>
					<image class="close" src="../../static/image/scclose.png" @tap="close(index)" v-if="zhuangtai == -1"></image>
				</view>
				<view class="dianji" @tap="upload()" v-if="imgpth.length != 4 && zhuangtai == -1">
					<image src="../../static/image/scimages.png"></image>
					<view class="zhangshu">{{imgpth.length}}/4</view>
				</view>
			</view>
		</view>
		<view class="xuanze" v-if="zhuangtai == 0">
			<view class="leixing">请选择事迹类型</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="title" disabled='zhuangtai == 0'>
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].title}}</text>
					<image src="../../static/image/sjrt.png"></image>
				</view>
			</picker>
		</view>
		<view class="mingcheng" v-if="zhuangtai == -1">
			<input type="tetx" v-model="title" placeholder="请输入事迹名称（如：助人为乐、见义勇为）" placeholder-style="color: #6A6A6A;"/>
		</view>
		<!-- <view class="miaoshu" v-if="zhuangtai == -1">
			<textarea class="mark" type="text"v-model="mark" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #999999;"></textarea>
		</view> -->
		<view class="box" v-if="zhuangtai == -1">
			<textarea maxlength="200"  class="duoh" type="text" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #676767;" v-model="pingjia"></textarea>
			<view class="xianzhi">{{pingjia.length}}/200</view>
			<view class="upload">
				<view class="yshangc" v-for="(item,index) in imgpth" :key="index">
					<image class="yjshang" :src="item"></image>
					<image class="close" src="../../static/image/scclose.png" @tap="close(index)" v-if="zhuangtai == -1"></image>
				</view>
				<view class="dianji" @tap="upload()" v-if="imgpth.length != 4">
					<image src="../../static/image/scimages.png"></image>
					<view class="zhangshu">{{imgpth.length}}/4</view>
				</view>
			</view>
		</view>
		<view class="xuanze" v-if="zhuangtai == -1">
			<view class="leixing">请选择事迹类型</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="title">
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].title}}</text>
					<image src="../../static/image/sjrt.png"></image>
				</view>
			</picker>
		</view>
		<view class="mingcheng" v-if="zhuangtai == 1">
			<input type="tetx" v-model="title" disabled='zhuangtai == 0' placeholder="请输入事迹名称（如：助人为乐、见义勇为）" placeholder-style="color: #6A6A6A;"/>
		</view>
		<view class="box" v-if="zhuangtai == 1">
			<!-- <textarea class="mark" type="text" disabled='zhuangtai == 0' v-model="mark" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #999999;"></textarea> -->
			<textarea maxlength="200" disabled='zhuangtai == 0' class="duoh" type="text" placeholder="请简要描述事件经过，管理员会帮您做事迹审核" placeholder-style="color: #676767;" v-model="pingjia"></textarea>
			<view class="xianzhi">{{pingjia.length}}/200</view>
			<view class="upload">
				<view class="yshangc" v-for="(item,index) in imgpth" :key="index">
					<image class="yjshang" :src="item" @tap="clickimg()"></image>
					<image class="close" src="../../static/image/scclose.png" @tap="close(index)" v-if="zhuangtai == -1"></image>
				</view>
				<view class="dianji" @tap="upload()" v-if="imgpth.length != 4 && zhuangtai == -1">
					<image src="../../static/image/scimages.png"></image>
					<view class="zhangshu">{{imgpth.length}}/4</view>
				</view>
			</view>
		</view>
		<view class="xuanze" v-if="zhuangtai == 1">
			<view class="leixing">请选择事迹类型</view>
			<picker class="gather" @change="anjianChange1" :value="index1" :range="array1" range-key="title" disabled='zhuangtai == 0'>
				<view class="flex-row">
					<text class="xiangmu">{{array1[index1].title}}</text>
					<image src="../../static/image/sjrt.png"></image>
				</view>
			</picker>
		</view>
		<view class="jieguo" v-if="zhuangtai == -1">
			<view>{{reject}}</view>
		</view>
		<view class="btn" v-if="zhuangtai == 0" disabled="disabled">审核中</view>
		<view class="btn" @tap='submit()' v-if="zhuangtai == -1">重新提交</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				title:'',
				mark:'',
				title2:'',
				mark2:'',
				array1: [],
				index1: 0,
				score:'',
				id:'',
				zancun:'',
				zhuangtai:'',
				reject:'',
				pingjia:'',
				imgpth:[],
				upimage:[],
				id:'',
				show:false
			}
		},
		onLoad(p) {
			let that = this
			that.id = p.id,
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/organization/scorelist',
				method:'GET',
				success(res) {
					// console.log(res.data.data)
					that.array1 = res.data.data
					uni.request({
						url:'https://jiafeng.boyaokj.cn/api/organization/shijiDetail',
						method:'GET',
						data:{
							id:that.id
						},
						success(res) {
							// console.log(res.data.data.images.replace(/\[|]/g, '').split(","))
							console.log(JSON.parse(res.data.data.images))
							console.log(res.data.data)
							that.title = res.data.data.title,
							that.pingjia = res.data.data.content,
							that.index1 = that.array1.findIndex(item => item.id === res.data.data.score_id)
							that.zhuangtai = res.data.data.status
							that.reject = res.data.data.reject
							that.imgpth = JSON.parse(res.data.data.images)
							// console.log()
						}
					})
				}
			})
		},
		onShow() {
			let that = this
			// console.log(this.title)
		},
		methods:{
			clickimg(){
				uni.previewImage({
					urls:this.imgpth,
					indicator:'default',
					longPressActions:true,
				})
			},
			upload(){
				let that = this
							let file = uni.chooseImage({
								count: 4, //默认9
								sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
								sourceType: ['album','camera'], //从相册选择、摄像头
								success: function(res) {
									that.imgpth = res.tempFilePaths
								},
							});
							that.show = !that.show
						},
						close(index){
							this.imgpth.splice(index,1);
						},
			anjianChange1(e) {
				// this.score = this.array1[e.detail.value].id
				this.index1 = e.detail.value;
			},
			submit(){
				let that = this
				let an = []
				console.log(that.id)
				if(that.show){
					for(let i in that.imgpth){
						console.log(that.imgpth[i])
						uni.uploadFile({
							url:'https://jiafeng.boyaokj.cn/api/file/upload',
							filePath: that.imgpth[i],
							name: 'file',
							success(res) {
								an.push(JSON.parse(res.data).data.url)
								that.upimage = an
								if(an.length == that.imgpth.length){
									console.log(JSON.stringify(an))
									uni.request({
										url:'https://jiafeng.boyaokj.cn/api/organization/editShiji',
										method:'POST',
										data:{
											user_id:uni.getStorageSync('userInfo').id,
											title:that.title,
											content:that.pingjia,
											score_id:that.array1[that.index1].id,
											images:JSON.stringify(an),
											id:that.id
										},
										success(res) {
											if(res.statusCode == 200){
												uni.showToast({
													title: '重新提交审核成功',
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
										}
									})
								}
							},fail(){
								console.log("错了")
							}
						})
					}
				}else{
					uni.request({
						url:'https://jiafeng.boyaokj.cn/api/organization/editShiji',
						method:'POST',
						data:{
							user_id:uni.getStorageSync('userInfo').id,
							title:that.title,
							content:that.pingjia,
							score_id:that.array1[that.index1].id,
							images:JSON.stringify(that.imgpth),
							id:that.id
						},
						success(res) {
							if(res.statusCode == 200){
									uni.showToast({
										title: '重新提交审核成功',
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
						}
					})
				}
				
			},
			// submit2(){
			// 	let that = this
			// 	uni.request({
			// 		url:'https://jiafeng.boyaokj.cn/api/organization/editShiji',
			// 		method:'POST',
			// 		data:{
			// 			user_id:uni.getStorageSync('userInfo').id,
			// 			title:that.title,
			// 			content:that.mark,
			// 			score_id:that.array1[that.index1].id,
			// 			id:that.id
			// 		},
			// 		success(res) {
			// 			uni.showToast({
			// 				title: '重新提交审核成功',
			// 				duration:1000
			// 			})
			// 			setTimeout(function() {
			// 			uni.switchTab({
			// 				url:'./index'
			// 			})
			// 			},1000)
			// 		}
			// 	})
			// }
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
		font-size: 28rpx;
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
