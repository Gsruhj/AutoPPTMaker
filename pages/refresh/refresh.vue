<template>
	<view class="content">
        <view class="uni-padding-wrap uni-common-mt">
            <view class="progress-box">
                <progress :percent="pgList[0]" show-info activeColor="#10AEFF" stroke-width="3" />
            </view>
            <view class="progress-control">
                <button type="primary" @click="setProgress">设置进度</button>
                <button type="warn" @click="clearProgress">清除进度</button>
            </view>
        </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'refresh',
				pgList: [ 0]
			}
		},
		methods: {
	        setProgress() {
	            this.pgList = [60]
	        },
	        clearProgress() {
	            this.pgList = [0]
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
		background-color: #ffffff;
		height: 100%;
		width: 100%;
	}
	
    .progress-box {
        height: 50rpx;
        margin-bottom: 60rpx;
    }
    .uni-icon {
        line-height: 1.5;
    }
    .progress-cancel {
        margin-left: 40rpx;
    }
    
    .progress-control button{
        margin-top: 20rpx;
    }
	
	.uni-padding-wrap{
		/* width:690rpx; */
		padding:0 30rpx;
	}
	.uni-common-mt{
		margin-top:300rpx;
	}

</style>
