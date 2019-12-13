<template>
	<view class="devinfo">
		<view class="topbox">
			<view>
				<view>设备号:</view>
				<view>{{devid}}</view>
			</view>
		</view>
		<view class="topbox2">
			<view>
				<view >光强:</view>
				<view >{{illumination}}</view>
			</view>
		</view>
		<view class="topbox">
			<view>
				<view>噪音:</view>
				<view>{{sound}}</view>
			</view>
		</view>
		<view class="topbox2">
			<view>
				<view>温度:</view>
				<view>{{termperature}}</view>
			</view>
		</view>
		<view class="topbox">
			<view>
				<view>湿度:</view>
				<view>{{humidity}}</view>
			</view>
		</view>
		<view class="uni-padding-wrap uni-common-mt">
			<button class="button"   @tap="getInfo">刷新数据</button>
		</view>
	</view>
</template>

<script>
	export default {
		components: {},
		data() {
			return {
				devid:'477077ac-04d3-4574-aea5-b7ca98b1415e',
				devinfo:null,
				userinfo:null,
				illumination:666,//光照
				sound:233,//噪音
				LED:true,
				signal:4,//信号强度
				termperature:25,
				humidity:30,
			}
		},
		onLoad:function(e){
			
		},
		methods: {
			getInfo(){
				uni.showLoading({
					title: '数据加载中...',
					mask: false
				});
				uni.request({
					url: this.globalVal.default_url.devInfo,
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
								this.sound = val.V;
								this.illumination = val.L
								console.log("温度: "+val.T+"C"+" 湿度: "+val.H+"%"+" 噪音: "+val.V+"%"+" 光强: "+val.L+"%");
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
		padding: 15px;
		height: 40px;
		margin: 9px;
		
		border-radius: 15px;
	}
	
	.topbox2{
		background: #66AAFF;
		padding: 15px;
		height: 40px;
		margin: 9px;
		border-radius: 15px;
	}
	.content{
		height: calc(100% - 10px);
	}
	.uni-tab{
		height: calc(100% - 210px); 
	}
	.button {
		margin-top: 30upx;
	    margin-left: 0;
	    margin-right: 0;
		margin-bottom: 10px;
		background: #FFB400;
		color: #000000;
		
	}
	@import "./style/devinfo.css";
	@media ( min-width : 750px) {
		.top{color:#f00}
	}
</style>
