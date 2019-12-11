<template>
	<view class="devinfo">
		<view class="topbox">
			<view>
				<view>设备号:{{devid}}</view>
			</view>
		</view>
		<view class="topbox2">
			<view>
				<view class="topbox_text">光强:{{illumination}}</view>
			</view>
		</view>
		<view class="topbox">
				<view>
					<view class="topbox_text">噪音:{{sound}}</view>
				</view>
		</view>
		<view class="topbox2">
				<view>
					<view class="topbox_text">LED灯:{{LED}}</view>
				</view>
		</view>
		<view class="topbox">
				<view>
					<view class="topbox_text">信号强度:{{signal}}</view>
				</view>
		</view>
	</view>
</template>

<script>
	export default {
		components: {},
		data() {
			return {
				devid:'477077ac-04d3-4574-aea5-b7ca98b1415e',
				illumination:666,//光照
				sound:233,//噪音
				LED:true,
				signal:4,//信号强度
				termperature:25,
				humidity:30,
			}
		},
		onLoad:function(e) {

		},
		methods: {
			getInfo(){
				uni.showLoading({
					title: '数据加载中...',
					mask: false
				});
				uni.request({
					url: this.globalVal.default_url.show,
					method: 'POST',
					data: {
						deviceId:this.devid
					},
					success: res => {
						console.log(res);
						if(200 == res.statusCode){
							this.devinfo = JSON.stringify(res.data);
							if(undefined == res.data.error_code){
								this.userinfo = res.data.services[0].data.infostring;
								let val = JSON.parse(this.userinfo);
								this.termperature = val.T;
								this.humidity = val.H;
								
								console.log("温度: "+val.T+"C"+" 湿度: "+val.H+"%");
							}
							
						}
					},
					fail: () => {},
					complete: () => {
						uni.hideLoading();
					}
				});
			}
		}
	}
</script>

<style>
	
	.top{
		color:#fff;
		height:30px;
		text-align: center;
	}
	.devinfo{
		background:#FFFFFF;
		color:#000000;
		height: 100%;
	}
	
	.topbox{
		background: #8A6DE9;
		padding: 20px;
		height: 40px;
		margin: 9px;
	}
	
	.topbox2{
		background: #4CD964;
		padding: 20px;
		height: 40px;
		margin: 9px;
	}
	.content{
		height: calc(100% - 10px);
	}
	.uni-tab{
		height: calc(100% - 210px); 
	}
	@import "./style/devinfo.css";
	@media ( min-width : 750px) {
		.top{color:#f00}
	}
</style>
