<template>
	<view class="content">
		<uni-nav-bar left-icon="gear" title="uni-app" fixed="true" @clickLeft="showDrawer">	
		</uni-nav-bar>		
		<text class="logo-font">Auto PPT</text>
		<view class="image-area">
			<image class="logo-begin" src="../../static/logo/transparent.png" @tap="begin"></image>
		</view>
		
		<uni-drawer ref="showLeft" mode="left" :mask-click="true">
		    <scroll-view style="height: 100%;" scroll-y="true">
				<text>\n</text>
				<view class="text-font">设置界面</view>
				<view class="horizontal"></view>
				<button type="default" size="default" @tap="login">登录</button>
				<button type="default" size="default">打卡</button>
				<text>\n\n</text>
		        <button @click="closeDrawer" type="warn" size="default" class="button-position">关闭设置</button>
		    </scroll-view>
		</uni-drawer>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				
			}
		},
		onLoad() {

		},
		methods: {
			begin(){
				uni.navigateTo({
					url:"../basic_info/basic_info"
				})
			},
			
			showDrawer() {
			    this.$refs.showLeft.open();
			},
			closeDrawer() {
			    this.$refs.showLeft.close();
			},
			
			login(){
				uni.login({
				  provider: 'weixin',
				  success: function (loginRes) {
				    console.log(loginRes.authResult);
				    // 获取用户信息
				    uni.getUserInfo({
				      provider: 'weixin',
				      success: function (infoRes) {
				        console.log('用户昵称为：' ,  infoRes.userInfo.nickName);	//String 用户昵称
						console.log('该服务商唯一用户标识：' ,infoRes.userInfo.openId);	//自用  String	该服务商唯一用户标识
						console.log('用户头像：' ,infoRes.userInfo.avatarUrl);	//  String	用户头像
						console.log('校验用户信息：' ,infoRes.signature);	//使用 sha1( rawData + sessionkey ) 得到字符串，用于校验用户信息。
						//user    data
				      }
				    });
				  }
				});
			},
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		position: absolute;
		flex-direction: column;
		align-items: center;
		background-color: #3fd5d5;
		height: 100%;
		width: 100%;
	}
	.horizontal {
		display: flex;
	    width: 100%;  
	    height: 2rpx;  
	    background-color: #B3B0B3;  
	} 
	
	.logo-font{
		display: block;
		font-size: 400%;
		color: #4bffff;
	}
	
	.image-area{
		display: flex;
		flex-direction: row;
		position: relative;
		height: 100%;
		width: 100%;
		align-items: center;
		justify-content: center;
	}
	
	.logo-begin{
		width: 150px;
		height: 150px;
	}

	.text-font{
		margin-left: 25rpx;
		margin-bottom: 15rpx;
		font-weight: 400;
		font-size: 150%;
	}
	.button-position{
		
	}
</style>
