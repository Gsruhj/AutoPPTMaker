<template>
	<view class="content">
		<view :animation="animationData" class="image-area">
			<image src="../../static/refresh.png"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'refresh',
				animationData: {}
			}
		},
		
		onShow: function(){
		    var animation = uni.createAnimation({
				duration: 1000,
				timingFunction: 'ease',
		    })
		    this.animation = animation
		    animation.rotateZ(180).step()
		    this.animationData = animation.export()
		    setTimeout(function() {
				animation.translate(30).step()
				this.animationData = animation.export()
		    }.bind(this), 1000)
		},
		methods: {
			rotateAndScale: function () {
				// 旋转并导出动画数据
				this.animation.rotateZ(180).step()
				this.animationData = this.animation.export()
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

</style>
