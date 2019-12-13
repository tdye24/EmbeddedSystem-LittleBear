<template>
	<view class="uni-common-mt">
		<view class="logo-content">
			<view v-if="lampstatus===1"><image class="logo" src="/static/lamp-on.png"></image></view>
			<view v-else><image class="logo" src="/static/lamp-off.png"></image></view>
			
		</view>
		<view class="uni-form-item uni-column">
			<view class="with-fun">
				<input class="uni-input inputText" placeholder="输入设备ID号" type="text" :value="devid" maxlength="36" @input="devIdInput"/>
			</view>
		</view>
		
		<view class="line"></view>
		<view class="uni-padding-wrap uni-common-mt">
			<button class="button"   @tap="lightUp">亮灯30秒</button>
			<button class="button"   @tap="lightsOff">灭灯</button>
			<button class="button"  @tap="alarm">蜂鸣</button>
		</view>
	</view>
</template>

<script>export default {
		data() {
			return {
				devid:'43132880-90cb-45e7-b4f9-f3f0d1670a82',
				cmdcode:0,
				cmdstr:'',
				cntDown:null,
				maxTime:0,
				btnAddDisable:true,
				lampstatus:0,
			}
		},
		onLoad:function (e) {
			
		},
		methods:{
			devIdInput(e){
				console.log("devIdInput");
				this.devid = e.target.value;
				if(36 == this.devid.length){
					this.btnAddDisable = false;
				}else{
					this.btnAddDisable = true;
				}
			},
			lightUp(){
				this.lampstatus=1;
				console.log(this.lampstatus);
				this.cmdcode = 3;
				this.cmdstr = 'on';
				console.log("sendCmd");
				if(0 != this.maxTime){
					console.log("sendCmd busy");
					return;
				}
				//{"cmdstring":"{"L1":0,"L2":0}","cmdlen":15,"cmdcode":3}
				let cmdpara = {
					cmdstring:this.cmdstr,
					cmdlen:this.cmdstr.length,
					cmdcode:this.cmdcode
				}
				let cmdstr = JSON.stringify(cmdpara);
				console.log("cmdstr:"+cmdstr);
				
				uni.request({
					url: this.globalVal.default_url.devCmd,
					method: 'POST',
					data: {
						deviceId:this.devid,
						cmdInfo:cmdstr
					},
					success: res => {
						uni.showToast({
							title: '命令下发成功!请检查设备端',
							icon:"none",
							duration:3000
						});
						this.btnAddDisable = true;
						this.maxTime = 60;
						this.countDownFun();
					},
					fail: () => {},
					complete: () => {}
				});
			},
			lightsOff(){
				this.lampstatus=0;
				console.log(this.lampstatus);
				this.cmdcode=0;
				this.cmdstr = 'off';
				console.log("sendCmd");
				if(0 != this.maxTime){
					console.log("sendCmd busy");
					return;
				}
				//{"cmdstring":"{"L1":0,"L2":0}","cmdlen":15,"cmdcode":3}
				let cmdpara = {
					cmdstring:this.cmdstr,
					cmdlen:this.cmdstr.length,
					cmdcode:this.cmdcode
				}
				let cmdstr = JSON.stringify(cmdpara);
				console.log("cmdstr:"+cmdstr);
				
				uni.request({
					url: this.globalVal.default_url.devCmd,
					method: 'POST',
					data: {
						deviceId:this.devid,
						cmdInfo:cmdstr
					},
					success: res => {
						uni.showToast({
							title: '命令下发成功!请检查设备端',
							icon:"none",
							duration:3000
						});
						this.btnAddDisable = true;
						this.maxTime = 60;
						this.countDownFun();
					},
					fail: () => {},
					complete: () => {}
				});
			},
			alarm(){
				
			},
			cmdCodeInput(e){//命令码
				console.log("cmdCodeInput");
				this.cmdcode = e.target.value;
				if(0 < this.cmdcode.length){
					this.btnAddDisable = false;
				}else{
					this.btnAddDisable = true;
				}
			},
			cmdStrInput(e){//命令字符
				console.log("cmdStrInput");
				this.cmdstr = e.target.value;
				if(0 < this.cmdstr.length){
					this.btnAddDisable = false;
				}else{
					this.btnAddDisable = true;
				}
			},
			sendCmd(){
				console.log("sendCmd");
				if(0 != this.maxTime){
					console.log("sendCmd busy");
					return;
				}
				//{"cmdstring":"{"L1":0,"L2":0}","cmdlen":15,"cmdcode":3}
				let cmdpara = {
					cmdstring:this.cmdstr,
					cmdlen:this.cmdstr.length,
					cmdcode:this.cmdcode
				}
				let cmdstr = JSON.stringify(cmdpara);
				console.log("cmdstr:"+cmdstr);
				
				uni.request({
					url: this.globalVal.default_url.devCmd,
					method: 'POST',
					data: {
						deviceId:this.devid,
						cmdInfo:cmdstr
					},
					success: res => {
						uni.showToast({
							title: '命令下发成功!请检查设备端',
							icon:"none",
							duration:3000
						});
						this.btnAddDisable = true;
						this.maxTime = 60;
						this.countDownFun();
					},
					fail: () => {},
					complete: () => {}
				});
			},
			countDownFun(){
				console.log("countDown start...");
				this.cntDown = setInterval(()=>{
					if(0 == this.maxTime){
						this.btnInfo = "命令下发"
						clearInterval(this.interval);
						this.interval = null;
						this.btnAddDisable = false;
						return;
					}else{
						this.maxTime--;
						this.btnInfo = this.maxTime+"秒";
					}
					console.log(this.btnInfo);
				},1000);
			}
		},
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
	.button {
		margin-top: 30upx;
	    margin-left: 0;
	    margin-right: 0;
		background: #8A6DE9;
		color: #000000;
	}
	
</style>
