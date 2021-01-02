<template>
	<view class="content">
		<text class="logo-font">Auto PPT</text>
		<view class="image-area">
			<image class="logo-begin" src="../../static/logo/begin.png" @tap="begin"></image>
		</view>
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
