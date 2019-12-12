<template>
	<view class="uni-common-mt">
		<view class="logo-content">
			<image class="logo" src="/static/dev.png" @tap="scanDev"></image>
		</view>
		
		<view class="uni-form-item uni-column">
			<view class="with-fun">
				<input class="uni-input inputText" placeholder="输入模组IMEI号" type="number" :value="imei" maxlength="15" @input="imeiInput"/>
			</view>
		</view>
		
		<view class="line"></view>
		<view class="uni-padding-wrap uni-common-mt">
			<button class="button"  @tap="regDev">注册添加设备</button>
		</view>
		
		<view class="textbox">
			<view>
				设备号:
			</view>
			<view>
				{{devid}}
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imei:null,
				devid:'477077ac-04d3-4574-aea5-b7ca98b1415e',
				btnAddDisable:true,
				btnInfoDisable:true
			}
		},
		onLoad() {

		},
		methods: {
			scanDev(){
				uni.scanCode({
					success: (res) => {
						var result;
						console.log('条码类型：' + res.scanType);
						console.log('条码内容：' + res.result);
						if(res.result.length>15){
							result = res.result.substring(0,15);
						}else{
							result = res.result;
						}
						console.log('result：' + result);
						this.imei = result;
						this.btnAddDisable = false;
					},
					fail: (err) => {
						console.log(err);
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = res.authSetting['scope.camera'];
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: this.i18n.content_note.text_app_name+'需要使用您的相机，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				});
			},
			imeiInput(e){
				console.log("imeiInput");
				this.imei = e.target.value;
				if(15 == this.imei.length){
					this.btnAddDisable = false;
				}else{
					this.btnAddDisable = true;
				}
			},
			regDev(){
				console.log("regDev URL:"+this.globalVal.default_url.devReg);
				uni.showLoading({
					title: '注册中...',
					mask: false
				});
				var errcode = 1;
				var errmsg = "失败";
				uni.request({
					url: this.globalVal.default_url.devReg,
					method: 'POST',
					data: {
						imei:this.imei
					},
					success: res => {
						console.log(res);
						if(200 == res.statusCode){
							errcode = res.data.errcode;
							if(0 == errcode){
								errmsg = "注册成功";
								this.devid = res.data.deviceId;
								this.btnInfoDisable = false;
							}else{
								errmsg = res.data.errmsg;
							}
						}
					},
					fail: () => {},
					complete: () => {
						uni.hideLoading();
						uni.showToast({
							title: errmsg,
							icon:"none",
							duration:2500
						});
					}
				});
			}
			
		}
	}
</script>

<style>
	.logo-content {
		text-align: center;
		margin-top: 200upx;
		margin-bottom: 100upx;
	}
	
	.logo {
		height: 120upx;
		width: 120upx;
	}
	
	.line {
		width: 90%;
		height: 1px;
		margin-left: 30upx;
		margin-right: 30upx;
		background-color: #cccccc;
		margin-top: 1px;
	}
	
	.inputText {
		margin-left: 30upx;
	}
	.devIdText{
		margin-left: 30upx;
		margin-right: 30upx;
	}
	.button {
		margin-top: 30upx;
	    margin-left: 0;
	    margin-right: 0;
		background: #8A6DE9;
		color: #000000;
	}
	.textbox{
		background: #66AAFF;
		padding: 10px;
		height: 80px;
		margin: 9px;
		border-radius: 10px;
	}
</style>
