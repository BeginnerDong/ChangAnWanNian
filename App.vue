
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
			wx.getSystemInfo({
			  success: e => {
			    this.globalData.statusBar = e.statusBarHeight; //状态栏高度
			    let custom = wx.getMenuButtonBoundingClientRect();//菜单按钮
			    this.globalData.custom = custom;
			    this.globalData.customBar = custom.bottom + custom.top - e.statusBarHeight;
			    //计算得到定义的状态栏高度
			  }
			})
		},
		globalData:{},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/* 富文本样式 */
	@import "/assets/style/quill.css";
	
	
	/* 公共样式 */
	@import "/common/css/common.css";
	/* 公共样式 */
	@import "/common/css/main.css";
	
	uni-swiper .uni-swiper-dots-horizontal{line-height: 16rpx;bottom: 5px;}
	
	uni-tabbar .uni-tabbar__icon{width: 44rpx;height: 44rpx;}
	uni-page-head .uni-page-head__title{font-weight: normal;}
	/* uni-swiper .uni-swiper-dot{width: 12rpx;height: 6rpx;border-radius: 6rpx;background-color: #c5c5c5;}*/
	uni-swiper .uni-swiper-dot-active{width: 28rpx;border-radius: 10rpx;} 
	.ql-editor img{max-width: 100%;margin: 0 auto;}
	
	/* .page-head{margin-top: var(--status-bar-height);} */
</style>
