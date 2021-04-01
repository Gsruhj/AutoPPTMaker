<template>
	<view>
		<view class="horizontal"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'refresh'
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		position: absolute;
		flex-direction: column;
		align-items: center;
		background-color: #ffffff;
		height: 100%;
		width: 100%;
	}
	
	.logo-font{
		display: block;
		font-size: 400%;
		color: #ffffff;
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
	
	.image-area{
			  position: relative;
			  display: flex;
			  align-items: center;
			  justify-content: center;
			  width: 50%;
			  height: 50%;
	}
	.info-header{
		display: flex;
		flex-direction: column;
		align-items: center;
		background-color: #ffffff;
		height: 15%;
		width: 15%;
	}

</style>
