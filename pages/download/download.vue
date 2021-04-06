<template>
	<view class="content">
		<view class="table">
			<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">根据自然语言处理生成的PPT</text>
			<view class="horizontal"></view>
			<view class="image-area">
				<image class="download-file" src="../../static/download.png"  @click="downloadFile"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'choose/previewImage',
				fileList:[],
			}
		},
		methods: {
			downloadFile(){
				this.fileList=[];
				uni.chooseImage({
				  success: function (res) {
				    var tempFilePaths = res.tempFilePaths;
				    uni.saveFile({
				      tempFilePath: tempFilePaths[0],
				      success: function (res) {
				        var savedFilePath = res.savedFilePath;
				      }
				    });
				  }
				});
			},
		}
	}
</script>

<style>
	.content{
		display: flex;
		flex-direction: column;
		position: absolute;
		width: 524px;
		height: 700px;
	}
	
	.horizontal {
		display: flex;
	    width: 100%;  
	    height: 2rpx;  
	    background-color: #B3B0B3;  
	}
	
	.table{
		flex-direction: column;
		position: relative;
		width: 100%;
		height: 100%;
		background-color: #f1f1f1;
	}
	
	.input-placeholder{
		color: #bcbcbc;
		font-size: 125%;
	}
	
	.image-area{
			  position: relative;
			  display: flex;
			  align-items: center;
			  justify-content: center;
			  width: 100%;
			  height: 30%;
	}
	
	.download-file{
			  width: 500rpx;
			  height: 500rpx;
	}

</style>
