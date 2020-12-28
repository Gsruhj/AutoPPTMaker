<template>
	<view class="content">
		<view class="info-header">
			<text class="header-logo">make your own PPT</text>
			<view class="header-text-area">
				<text class="header-text-selected">基本信息</text>
				<text class="header-text">文档导入</text>
				<text class="header-text">选择模板</text>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.content{
		display: flex;
		flex-direction: column;
	}
	
	.info-header{
		display: flex;
		flex-direction: column;
		align-items: center;
		background-color: #3fd5d5;
		position: absolute;
		height: 15%;
		width: 100%;
	}
	
	.header-logo{
		display: block;
		justify-content: center;
		font-size: 100%;
		color: #4bffff;
	}
	
	.header-text-area{
		display: flex;
		flex-direction: row;
		align-items: flex-end;
		justify-content: center;
		height: 100%;
		width: 100%;
		
	}
	
	.header-text{
		color: #000000;
		margin-left: 25rpx;
		margin-bottom: 15rpx;
		font-weight: 400;
		font-size: 150%;
		
	}
	
	.header-text-selected{
		color: #e6e6e6;
		margin-left: 25rpx;
		margin-bottom: 15rpx;
		font-weight: 200;
		font-size: 150%;
		text-decoration:underline;
	}
</style>
