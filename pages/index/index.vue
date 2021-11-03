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
				
				<view class='header'>
					<image src='../../static/wx_login.png'></image>
				</view>
				<view class='mycontent'>
					<view>申请获取以下权限</view>
					<text>获得你的公开信息(昵称，头像、地区等)</text> 
				</view>
				<view>
					<button class="sys_btn" open-type="getUserInfo" lang="zh_CN" @getuserinfo="appLoginWx">{{loginInfo.openid != "" && loginInfo.openid != undefined ? "已授权" : "小程序授权"}}</button>
				</view>

				<text>\n</text>
				<button type="default" size="default">打卡</button>
				<button type="default" size="default" @click="getstore">商城</button>
		        <button @click="closeDrawer" type="warn" size="default" class="button-position">关闭设置界面</button>
		    </scroll-view>
		</uni-drawer>
	</view>
</template>

<script>
	import helper from '../../common/helper.js'; 
	export default {
		data() {
			return {
				title: 'Hello',
				

				code: "",
				SessionKey: '',
				encryptedData: "",
				iv: "",
				OpenId: '',
				nickName: null,
				gender: 0,
				avatarUrl: null,
				isCanUse: uni.getStorageSync('isCanUse'), //默认为true  记录当前用户是否是第一次授权使用的		
						
				AppId:"******************",
				AppSecret:"********************************",
				username: null,
				
			}
		},
		onLoad() {

			var _this=this
			uni.getSetting({
				success(res) {
					console.log("授权：", res);
					if (!res.authSetting['scope.userInfo']) {
						//这里调用授权
						console.log("当前未授权");
					} else {
						//用户已经授权过了
						console.log("当前已授权");
						_this.login();
					}
				}
			})
			
		},
		methods: {
			begin(){
				uni.navigateTo({
					url:"../basic_info/basic_info?username="+this.username
				})
			},
			
			showDrawer() {
			    this.$refs.showLeft.open();
			},
			closeDrawer() {
			    this.$refs.showLeft.close();
			},
			getstore(){
				uni.navigateTo({
					url:"../store/store",
				})
			},
			
			appLoginWx() {
				var _self = this;
				// #ifdef MP-WEIXIN
				uni.getProvider({
					service: 'oauth',
					success: function(res) {
						if (~res.provider.indexOf('weixin')) {
							uni.login({
								provider: 'weixin',
								success: (res) => {
									_self.code = res.code;
									uni.getUserInfo({
										provider: 'weixin',
										success: (info) => { 	//这里请求接口
											console.log(res);
											console.log(info);
											_self.login();
										},
										fail: () => {
											uni.showToast({
												title: "微信登录授权失败-1",
												icon: "none"
											});
										}
									})
 
								},
								fail: () => {
									uni.showToast({
										title: "微信登录授权失败-2",
										icon: "none"
									});
								}
							})
 
						} else {
							uni.showToast({
								title: '请先安装微信或升级版本',
								icon: "none"
							});
						}
					}
				});
				//#endif
			},
			login()	 {
				let _this = this;
				uni.showLoading({
					title: '登录中...'
				});
 
				// 1.wx获取登录用户code
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log("登录", loginRes.code)
						_this.code = loginRes.code;
						if (!_this.isCanUse) {
							//非第一次授权获取用户信息
							uni.getUserInfo({
								provider: 'weixin',
								success: function(infoRes) {
									console.log('login用户信息：', infoRes.userInfo);
									//获取用户信息后向调用信息更新方法
									_this.nickName = infoRes.userInfo.nickName; //昵称
									_this.avatarUrl = infoRes.userInfo.avatarUrl; //头像
									_this.gender = infoRes.userInfo.gender;
									_this.updateUserInfo(); //调用更新信息方法
								},
								fail(err) {
									console.log("-----------" + err)
								}
							});
						}
						// 将用户登录code传递到后台置换用户SessionKey、OpenId等信息

 
 
						uni.hideLoading();
					},
				})
				var login_url = helper.websiteUrl+'/login';
				console.log('login_url',login_url);
				uni.request({
				    //url: 'http://127.0.0.1:8000/login', 
					url: login_url, 
					method:'POST',
				    data: {
				        "appId": this.AppId,
						"appSecret": this.AppSecret,
						"code":this.code
				    },
				    success: (res) => {
				        console.log(res.data);
				        this.text = 'request success';
						console.log("cookies:",res.cookies)
						//console.log("cookies:",res.cookies[0])
						
						this.username=res.cookies[0].match(/username=(\S*);/)[1];
						//this.username=res.cookies[0];
						console.log("username:",this.username)
				    }
				});
 
			},
			updateUserInfo() {
				var _this = this;
				var form = {
					appid: "你的微信小程序appid",
					secret: "需要在开发选项里面获取",
					js_code: "你登录拿取的code",
					grant_type: "authorization_code",//这个是写死的 就直接可以使用authorization_code
				}
 
				//good.wxget("https://api.weixin.qq.com/sns/jscode2session", form, function(data) {
					
				//});
			}

			
			
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


    .header {
        margin: 90rpx 0 90rpx 50rpx;
        border-bottom: 1px solid #ccc;
        text-align: left;
        width: 650rpx;
        height: 300rpx;
        line-height: 450rpx;
    }
 
    .header image {
        width: 200rpx;
        height: 200rpx;
    }
 
    .mycontent {
        margin-left: 25rpx;
        margin-bottom: 90rpx;
    }
 
    .content text {
        display: block;
        color: #9d9d9d;
        margin-top: 40rpx;
    }
 
    .bottom {
        border-radius: 80rpx;
        margin: 70rpx 50rpx;
        font-size: 35rpx;
    }

</style>
