<script>
import Vue from 'vue'
	export default {
		onLaunch: function() {
			console.log('App Launch')
			uni.getSystemInfo({  
        success:function(e){  
					console.log('e.statusBarHeight', e.statusBarHeight);
            Vue.prototype.statusBar = e.statusBarHeight  
            // #ifndef MP  
            if(e.platform == 'android') {  
                Vue.prototype.customBar = e.statusBarHeight + 50  
            }else {  
                Vue.prototype.customBar = e.statusBarHeight + 45  
            }  
            // #endif  

            // #ifdef MP-WEIXIN  
            let custom = wx.getMenuButtonBoundingClientRect()  
            Vue.prototype.customBar = custom.bottom + custom.top - e.statusBarHeight  
            // #endif  

            // #ifdef MP-ALIPAY  
            Vue.prototype.customBar = e.statusBarHeight + e.titleBarHeight  
            // #endif  
        }  
    })  
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import "uview-ui/index.scss";
	@import 'uni.scss'
</style>
