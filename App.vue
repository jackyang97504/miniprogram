<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			// 检查登录状态
			const isLoggedIn = uni.getStorageSync('isLoggedIn');
			const userInfo = uni.getStorageSync('userInfo');
			
			// 如果未登录或没有用户信息，直接跳转到登录页面
			if (!isLoggedIn || !userInfo) {
				uni.reLaunch({
					url: '/pages/login/login'
				});
				return;
			}
			
			// 检查登录是否过期（7天）
			const loginTime = userInfo.loginTime;
			const currentTime = new Date().getTime();
			const sevenDays = 7 * 24 * 60 * 60 * 1000;
			
			if (currentTime - loginTime >= sevenDays) {
				// 登录已过期，清除登录状态并跳转到登录页面
				uni.removeStorageSync('isLoggedIn');
				uni.removeStorageSync('userInfo');
				uni.reLaunch({
					url: '/pages/login/login'
				});
				return;
			}
			
			// 已登录且未过期，跳转到首页
			uni.reLaunch({
				url: '/pages/index/index'
			});
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
