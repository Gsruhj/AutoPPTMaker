<template>
	<view>
		<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">逻辑划分内容及各部分摘要</text>
		<view class="horizontal"></view>
		<view>
			<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper" @scrolltolower="lower"
				@scroll="scroll">
				<view class="uni-list" v-for="summ,index in summary" :key="summ.heading">>
					{{index}}
				    <view class="table-item">
				        <input class="input" :value="summ.heading" @input="input_heading"/>
				    </view>
					<view class="uni-textarea">
						<textarea :value="summ.name" @input="input_summary"/>
					</view>
					<view class="image-area">
						<image class="add_list" src="../../static/add.jpg"  @click="add_item_to_list(index)"></image>
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
				
				summary:[
					{
						name: "jack",
						heading:"one",
					},
					{
						name: "rose",
						heading:"two",
					},
					{
						name: "james",
						heading:"Three",
					},
					{
						name: "durant",
						heading:"Four",
					},
				],
				
				id:0,
				contents:null,
			}
		},
		onLoad(options) {
			console.log('转移的summary',option.summary);
			this.summary=options.summary;
			
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
			
			input_heading(e) {
				this.summary[index].heading = e.detail.value
			},
			input_summary(e) {
				this.summary[index].name = e.detail.value
			},
			
			add_item_to_list(index){
				//this.summary.push({name:"请添加标题",heading:"请添加摘要"})
				//this.summary.unshift({name:"请添加标题",heading:"请添加摘要"})
				this.summary.splice(index+1,0,{name:"请添加标题",heading:"请添加摘要"})
			},
			
			submitted(){
				let params = {					
					"summary":this.summary,
				};
				uni.request({
					url: 'http://127.0.0.1:8000/display',
					method: 'POST',
					data: params,
					success: (res)=>{
						console.log(res.data);
						this.contents=res.data.display;
					},
					fail: (err)=>{
						
					}
				}),
				
				uni.navigateTo({
					url:"../ppt/ppt?contents="+this.contents,
				});
				
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
	.uni-list-cell-pd {
		padding: 22rpx 30rpx;
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