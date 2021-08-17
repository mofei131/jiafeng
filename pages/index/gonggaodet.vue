<template>
	<view style="overflow: hidden;">
	<view class="about">
		<!-- <view v-html="content"></view> -->
		<rich-text  v-html = 'content'></rich-text>
	</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				content: '',
				id:''
			}
		},
		onLoad(p) {
			let that = this
			that.id = p.id
			uni.request({
				url:'https://jiafeng.boyaokj.cn/api/notice/detail',
				method:'GET',
				data:{
					id:that.id,
					user_id:uni.getStorageSync('userInfo').id
				},
				success(res) {
					// console.log(res.data.data.content)
					// that.content = res.data.data.data
					that.content = that.formatRichText(res.data.data.content)
				}
			})
		},
		methods:{
			formatRichText(html) {
					let newContent = html.replace(/<img[^>]*>/gi, function(match, capture) {
						match = match.replace(/style="[^"]+"/gi, '').replace(/style='[^']+'/gi, '');
						match = match.replace(/width="[^"]+"/gi, '').replace(/width='[^']+'/gi, '');
						match = match.replace(/height="[^"]+"/gi, '').replace(/height='[^']+'/gi, '');
						return match;
					});
					newContent = newContent.replace(/style="[^"]+"/gi, function(match, capture) {
						match = match.replace(/width:[^;]+;/gi, 'max-width:100%;').replace(/width:[^;]+;/gi, 'max-width:100%;');
						return match;
					});
					newContent = newContent.replace(/<br[^>]*\/>/gi, '');
					newContent = newContent.replace(/\<img/gi,
						'<img style="max-width:100%;height:auto;display:inline-block;margin:10rpx auto;"');
					return newContent;
				}
		}
	}
</script>

<style>
	page{
		background: #F4F7F7;
	}
	.about{
		width: 720rpx;
		margin: auto;
		border-radius: 10rpx;
		/* background: #fff; */
		padding: 20rpx;
		height: 100%;
	}
</style>
