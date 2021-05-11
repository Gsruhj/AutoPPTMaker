<template>
	<view>
		<text class="input-placeholder" style="margin: 15rpx;margin-bottom: 0rpx;color: #000000;">生成式摘要及逻辑框架</text>
		<view class="horizontal"></view>
		<view>
			<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper" @scrolltolower="lower"
				@scroll="scroll">
				<view class="uni-list" v-for="summ in summary" :key="summ.heading">>
				    <view class="table-item">
				        <input class="input" :placeholder="summ.heading" placeholder-class="input-placeholder" />
				    </view>
					<view class="uni-textarea">
						<textarea placeholder-style="color:#F76260" :placeholder="summ.name"/>
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
			
			submitted(){
				let params = {

				};
				uni.request({
					url: 'http://api.komavideo.com/news/list',
					method: 'POST',
					data: params,
					success: (res)=>{},
					fail: (err)=>{}
				}),
				
				uni.navigateTo({
					url:"../modify/modify",
				})
			},
		}
	}
</script>

<style>
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
	
	.input-placeholder{
		color: #bcbcbc;
		font-size: 125%;
	}
	
	/* textarea */
	.uni-textarea{
		width:100%;
		background:#FFF;
	}

</style>
