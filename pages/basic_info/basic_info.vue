<template>
	<view class="content">
		<view class="info-header">
			<text class="header-logo">Make Your Own PPT</text>
			<view class="header-text-area">
				<text class="header-text" :style="{color:choose_index==1?'#f4f4f4':'#000000'}" @click="jumpToBasicInfo">基本信息</text>
				<text class="header-text" :style="{color:choose_index==2?'#f4f4f4':'#000000'}" @click="jumpToDocLoad">文档导入</text>
				<text class="header-text" :style="{color:choose_index==3?'#f4f4f4':'#000000'}" @click="jumpToChoseTemplate">选择模板</text>
			</view>

		</view>
		<view class="table" :style="{display:choose_index==1?'flex':'none'}">
			<text class="input-placeholder" style="margin: 15rpx;color: #000000;">输入基本信息</text>
			<view class="horizontal"></view>
			<view class="table-item">
				<input class="input"  placeholder="展示标题" placeholder-class="input-placeholder" />
			</view>
			<view class="horizontal"></view>
			<view class="table-item">
				<input class="input" placeholder="演讲者" placeholder-class="input-placeholder" />
			</view>
			<view class="horizontal"></view>
			<view class="table-item">
				<input class="input" placeholder="生成页数" placeholder-class="input-placeholder" />
			</view>
			<view class="horizontal"></view>
			<text class="input-placeholder" style="margin: 15rpx;color: #000000;">选择您要插入的图片</text>

				<view class="uni-uploader__files">
					<block v-for="(image,index) in imageList" :key="index">
						<view class="uni-uploader__file">
							<image class="uni-uploader__img" :src="image" :data-src="image" @tap="previewImage"></image>
						</view>
					</block>
					<view class="uni-uploader__input-box">
						<view class="uni-uploader__input" @tap="chooseImage"></view>
					</view>
				</view>
			<view class="horizontal"></view>
			
		</view>
		
		<view class="table" :style="{display:choose_index==2?'flex':'none'}">
			<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">选择制作PPT依据的文本</text>
			<text class="input-placeholder" style="margin-left: 15rpx;color: #bcbcbc;font-size: 50%;">当前版本只支持txt格式</text>
			<view class="horizontal"></view>
			<view class="image-area">
				<image class="load-file" :src="loadFileFlag==false?'../../static/plus.png':'../../static/plus_selected.png'" @click="loadFile"></image>
			</view>
		</view>
		
		<view class="table" :style="{display:choose_index==3?'flex':'none'}">
			<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">选择制作PPT所用的模板</text>
			<view class="horizontal"></view>
			<view>
				<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper" @scrolltolower="lower"
					@scroll="scroll">
					<view class="uni-title uni-common-pl">模板1</view>
					<view class="template-image-area">
						<image class="template_size" src="../../static/template/鼠.png"></image>
					</view>
					<view class="horizontal"></view>
					<view class="uni-title uni-common-pl">模板2</view>
					<view class="template-image-area">
						<image class="template_size" src="../../static/template/牛.png"></image>
					</view>
					<view class="horizontal"></view>
					<view class="uni-title uni-common-pl">模板3</view>
					<view class="template-image-area">
						<image class="template_size" src="../../static/template/虎.png"></image>
					</view>
					<view class="horizontal"></view>
					<view class="uni-title uni-common-pl">模板4</view>
					<view class="template-image-area">
						<image class="template_size" src="../../static/template/兔.png"></image>
					</view>
				</scroll-view>
			</view>

			<view class="uni-list">
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">
						当前选择
					</view>
					<view class="uni-list-cell-db">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
					</view>
				</view>
			</view>

			<view>
				<button size="default" @tap="submitted">确定</button>
			</view>
		</view>
	</view>
</template>

<script>
	var sourceType = [
		['camera'],
		['album'],
		['camera', 'album']
	]
	var sizeType = [
		['compressed'],
		['original'],
		['compressed', 'original']
	]
	
	export default {
		data() {
			return {
				loadFileFlag:false,
				choose_index:1,
				title: 'choose/previewImage',
				imageList: [],
				fileList:[],
				sourceTypeIndex: 2,
				sourceType: ['拍照', '相册', '拍照或相册'],
				sizeTypeIndex: 2,
				sizeType: ['压缩', '原图', '压缩或原图'],
				countIndex: 8,
				count: [1, 2, 3, 4, 5, 6, 7, 8, 9],
				
				
				scrollTop: 0,
				old: {
					scrollTop: 0
				},
				
				array: ['模板1', '模板2', '模板3', '模板4'],
				index: 0
			}
		},
		onUnload() {
			this.imageList = [],
			this.fileList=[],
				this.sourceTypeIndex = 2,
				this.sourceType = ['拍照', '相册', '拍照或相册'],
				this.sizeTypeIndex = 2,
				this.sizeType = ['压缩', '原图', '压缩或原图'],
				this.countIndex = 8;
		},
		methods: {
			upper: function(e) {
				console.log(e)
			},
			lower: function(e) {
				console.log(e)
			},
			scroll: function(e) {
				console.log(e)
				this.old.scrollTop = e.detail.scrollTop
			},
			
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			
			submitted(){
				uni.navigateTo({
					url:"../refresh/refresh"
				})
			},
			
			
			loadFile(){
				this.fileList=[];
				uni.chooseFile({
					extension:['.txt'],
					success: (res) => {
						this.fileList = this.fileList.concat(res.tempFilePaths);
						if(this.fileList.length!=1){
							uni.showToast({
								icon:"none",
								title:"请只选择一个txt文件！"
							})
							this.fileList=[];
						}
						else{
							uni.showToast({
								icon:"success",
								title:"您已选择：  "+res.tempFiles[0].name
							});
							this.loadFileFlag=true;
						}
					},
				});
			},
			jumpToBasicInfo(){
				this.choose_index=1;
			},
			
			jumpToDocLoad(){
				this.choose_index=2;
			},
			
			jumpToChoseTemplate(){
				this.choose_index=3;
			},
			
			chooseImage: async function() {
			
				if (this.imageList.length === 9) {
					let isContinue = await this.isFullImg();
					console.log("是否继续?", isContinue);
					if (!isContinue) {
						return;
					}
				}
				uni.chooseImage({
					sourceType: sourceType[this.sourceTypeIndex],
					sizeType: sizeType[this.sizeTypeIndex],
					count: this.imageList.length + this.count[this.countIndex] > 9 ? 9 - this.imageList.length : this.count[this.countIndex],
					success: (res) => {
						this.imageList = this.imageList.concat(res.tempFilePaths);
					},
				})
			},
			isFullImg: function() {
				return new Promise((res) => {
					uni.showModal({
						content: "已经有9张图片了,是否清空现有图片？",
						success: (e) => {
							if (e.confirm) {
								this.imageList = [];
								res(true);
							} else {
								res(false)
							}
						},
						fail: () => {
							res(false)
						}
					})
				})
			},
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
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
	
	.info-header{
		display: flex;
		flex-direction: column;
		align-items: center;
		background-color: #3fd5d5;
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
	
	.table{
		flex-direction: column;
		position: relative;
		width: 100%;
		height: 100%;
		background-color: #f1f1f1;
	}
	
	.table-item{
		margin-top: 15rpx;
	}
	
	.input{
		margin-left: 15rpx;
		height: 100rpx;
		margin-top: 50rpx;
		margin-bottom: -20rpx;
	}
	
	.input-placeholder{
		color: #bcbcbc;
		font-size: 125%;
	}
	
	.horizontal {  
		display: flex;
	    width: 100%;  
	    height: 2rpx;  
	    background-color: #B3B0B3;  
	  } 
	  .uni-uploader__files {
	  	display: flex;
	  	flex-direction: row;

	  }
	  .uni-uploader__file {
	  	margin: 10rpx;
	  	width: 210rpx;
	  	height: 210rpx;
	  }
	  .uni-uploader__img {
	  	display: block;
	  	width: 210rpx;
	  	height: 210rpx;
	  }
	  
	  .uni-uploader__input-box {
	  	position: relative;
	  	margin:10rpx;
	  	width: 208rpx;
	  	height: 208rpx;
	  	border: 2rpx solid #D9D9D9;
	  }
	  .uni-uploader__input-box:before,
	  .uni-uploader__input-box:after {
	  	content: " ";
	  	position: absolute;
	  	top: 50%;
	  	left: 50%;
	  	-webkit-transform: translate(-50%, -50%);
	  	transform: translate(-50%, -50%);
	  	background-color: #D9D9D9;
	  }
	  .uni-uploader__input-box:before {
	  	width: 4rpx;
	  	height: 79rpx;
	  }
	  .uni-uploader__input-box:after {
	  	width: 79rpx;
	  	height: 4rpx;
	  }
	  .uni-uploader__input-box:active {
	  	border-color: #999999;
	  }
	  .uni-uploader__input-box:active:before,
	  .uni-uploader__input-box:active:after {
	  	background-color: #999999;
	  }
	  .uni-uploader__input {
	  	position: absolute;
	  	z-index: 1;
	  	top: 0;
	  	left: 0;
	  	width: 100%;
	  	height: 100%;
	  	opacity: 0;
	  }
	  
	  .load-file{
		  width: 300rpx;
		  height: 300rpx;
	  }
	  
	  .image-area{
		  position: relative;
		  display: flex;
		  align-items: center;
		  justify-content: center;
		  width: 100%;
		  height: 100%;
	  }
	  
	.scroll-Y {
	    height: 1000rpx;
	}
	  
	.template_size{
	 	width: 300rpx;
	  	height: 300rpx;
	}
	  
	.uni-input {
	  	height: 50rpx;
	  	padding: 15rpx 25rpx;
	  	line-height:50rpx;
	  	font-size:28rpx;
	  	background:#FFF;
	  	flex: 1;
	}
	.uni-list {
	  	background-color: #FFFFFF;
	  	position: relative;
	  	width: 100%;
	  	display: flex;
	  	flex-direction: column;
	}
	.uni-list-cell {
	  	position: relative;
	  	display: flex;
		flex-direction: row;
	  	justify-content: space-between;
	  	align-items: center;
	}
	.uni-list-cell-left {
	    white-space: nowrap;
	  	font-size:28rpx;
	  	padding: 0 30rpx;
	}
	.uni-list-cell-db{
		flex: 1;
	}
	
	.template-image-area{
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 50%;
	}
	  
</style>
