<template>
	<view>
		<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">PPT每页内容划分</text>
		<view class="horizontal"></view>
		<view>
			<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper" @scrolltolower="lower"
				@scroll="scroll">
				<view class="uni-list" v-for="content,index in contents" :key="content">
					{{index}}
					<view v-for="cont,cont_index in content" :key="cont">
						<view class="uni-textarea">
							<textarea :value="cont" @input="input_content"/>
						</view>
						<view class="image-area">
							<image class="add_list" src="../../static/add.jpg"  @click="add_item_to_list(index,cont_index)"></image>
						</view>
					</view>
					<view class="horizontal"></view>
				</view>
			</scroll-view>
			<view>
				<button size="default" @tap="submitted">确定</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				scrollTop: 0,
				old: {
					scrollTop: 0
				},
				
				contents:[
					[
						"one",
						"Four",
						"Three",
						"two",
					],
					[
						"rose",
						"lili",
					],
					[
						"jack",
						"james",
						"durant",
					],
				],
				
			}
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
			
			input_content(e) {
				this.contents[index][cont_index] = e.detail.value
			},
			add_item_to_list(index,cont_index){
				//this.summary.push({name:"请添加标题",heading:"请添加摘要"})
				//this.summary.unshift({name:"请添加标题",heading:"请添加摘要"})
				this.contents[index].splice(cont_index+1,0,"请添加摘要")
			},
			
			submitted(){
				
				uni.navigateTo({
					url:"../download/download",
				});
				
				
				let params = {
					"content":this.contents,
				};
				uni.request({
					url: 'http://api.komavideo.com/news/list',
					method: 'POST',
					data: params,
					success: (res)=>{},
					fail: (err)=>{}
				})

			},
			
			onShow() {
				//alert('触发了！')
				//.判断是否已连接
				this.checkOpenSocket();
			},
			 // 判断是否已连接
			checkOpenSocket () {
				let self = this;
				uni.sendSocketMessage({
					data: 'ping',
					success: (res) => {
						return;
					},
					fail: (err) => { 	// 未连接打开websocket连接
						self.openConnection(); 
					}
				});
			},
			openConnection () { 	// 打开连接
				uni.closeSocket(); 	// 确保已经关闭后再重新打开
				uni.connectSocket({
					url: 'wss://www.example.com/socket',
					method: 'POST',
					success(res) {
						console.log('连接成功 connectSocket=', res);
					},
					fail(err) {
						console.log('连接失败 connectSocket=', err);
					}
				});
				uni.onSocketOpen((res) => {
					console.log('连接成功', res);
				});
				// 打开成功监听服务器返回的消息
				uni.onSocketMessage(function (res) {
				  console.log('收到服务器内容：' + res.data);
				  this.summary=res.data;
				});
				uni.closeSocket();
			},
			
			
		}
	}
</script>

<style>
	.horizontal {
		display: flex;
	    width: 100%;  
	    height: 2rpx;  
	    background-color: #B3B0B3;  
	} 
	.input-placeholder{
		color: #bcbcbc;
		font-size: 125%;
	}
	.scroll-Y {
	    height: 100%;
	}
	
	.uni-list {
	  	background-color: #FFFFFF;
	  	position: relative;
	  	width: 100%;
	  	display: flex;
	  	flex-direction: column;
	}
	.table-item{
		margin-top: 15rpx;
	}
	/* textarea */
	.uni-textarea{
		width:100%;
		background:#FFF;
	}
	/*add item to list*/
	.image-area{
			  position: relative;
			  display: flex;
			  align-items: center;
			  justify-content: center;
			  width: 100%;
			  height: 30%;
	}
	.add_list{
		width: 150rpx;
		height: 150rpx;
	}

</style>
