<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
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
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	page{
		background: #F7F4F4;
	}
</style>
