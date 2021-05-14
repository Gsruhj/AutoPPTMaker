<template>
	<view class="content">
		<uni-nav-bar left-icon="gear" title="uni-app" fixed="true" @clickLeft="showDrawer">	
		</uni-nav-bar>		
		<text class="logo-font">Auto PPT</text>
		<view class="image-area">
			<image class="logo-begin" src="../../static/logo/transparent.png" @tap="begin"></image>
		</view>
		
		<uni-drawer ref="showRight" mode="left" :mask-click="false">
		    <scroll-view style="height: 100%;" scroll-y="true">
				<view v-for="item in 5" :key="item">可滚动内容 {{ item }}</view>
		        <button @click="closeDrawer" type="primary">关闭Drawer</button>
		    </scroll-view>
		</uni-drawer>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
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
			    this.$refs.showRight.open();
			},
			closeDrawer() {
			    this.$refs.showRight.close();
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

</style>
