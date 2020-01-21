<template>
	<div v-if="showAll">
		<div>
			<image style="width: 100%; display: block;padding-bottom:40px;" src="../../static/images/registered-img.png" mode="widthFix"/></image>
		</div>
		
		<div class="pub_editInfor pdlr4">
			<div class="item flexCenter">
				<div class="name"><image class="icon" src="../../static/images/registered-icon.png" /></div>
				<div class="rr bordB1">
					<input type="number"maxlength="11" value="" placeholder="请输入手机号码" v-model="submitData.login_name"/>
				</div>
			</div>
			<div class="item flexCenter">
				<div class="name"><image class="icon" src="../../static/images/registered-icon1.png" /></div>
				<div class="rr bordB1">
					<input type="text" value="" placeholder="请输入登录密码" v-model="submitData.password"/>
				</div>
			</div>
		</div>
		
		<div class="submitbtn" style="margin-top: 80px;">
			<button class="btn" type="button" @click="submit">登录</button>
		</div>
	</div>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('storeToken')) {
				uni.redirectTo({
					url: '/pages/business/business'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('storeToken', res.token);
							uni.setStorageSync('storeInfo', res.info);
							uni.redirectTo({
								url: '/pages/business/business'
							}) 
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.login(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
	};
</script>


<style>
	@import "../../assets/style/user.css";
	.pub_editInfor .item{ width: 90%;margin: 0 auto;line-height: 20px;padding-bottom: 10px; display: flex;justify-content: flex-start;box-sizing: border-box; align-items: center;}
	.pub_editInfor .item .name .icon{width: 20px; height: 20px; display: block;}
	.pub_editInfor .item .rr{ width: 90%;padding:10px 0;box-sizing: border-box;}
	.pub_editInfor .item .rr input{width: 100%;box-sizing: border-box;padding: 0px 10px;font-size: 26rpx;}
</style>

