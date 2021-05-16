<template>
	<view class="content">
		<view class="table">
			<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">PPT下载</text>
			<view class="horizontal"></view>
			
			<l-file ref="lFile" @up-success="onSuccess"></l-file>
			<view class="padding text-center">
				<view class="padding">
					<button @tap="onDownload">PPT下载</button>
				</view>
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
				ppt_url:null,
			}
		},
		onLoad(options) {
			this.ppt_url=options.ppt_url;
		},
		methods: {	
			onDownload(){
				/**
				* 保存到本地
				* type 非save为临时下载
				* customName 仅type=save生效 附件自定义名称需带后缀，自定义目录需以/结尾
				* DownloadOptions 仅type=save生效 可选参数(http://www.html5plus.org/doc/zh_cn/downloader.html#plus.downloader.DownloadOptions)
				* 默认下载到_downloads/files/ 可通过DownloadOptions自定义
				*/
				this.$refs.lFile.download({
					url:this.ppt_url, //必填，附件网络地址
					type:'save', //选填，非save为临时下载
					customName:'test.pptx',
					//customName:'自定义文件名需要带后缀.jpg',
					//...DownloadOptions直接写key:value 
					// 例如：
					method: 'GET'
				})
				.then(path=>{
					this.localPath = path;
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
		width: 100%;
		height: 100%;
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
			  width: 400rpx;
			  height: 400rpx;
	}

</style>
