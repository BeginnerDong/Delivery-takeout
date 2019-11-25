<template>
	<div>
		
		<div class="editLine pdlr4">
			<ul>
				<li class="item flex pdtb15 bordB1">
					<span class="ll">持卡人：</span>
					<div class="rr">
						<input class="edit" type="text" value="" placeholder="请输入持卡人的姓名">
					</div>
				</li>
				<li class="item flex pdtb15 bordB1">
					<span class="ll">手机号：</span>
					<div class="rr">
						<input class="edit" type="number" value="" placeholder="请输入您银行卡绑定的手机号" onkeyup="this.value=this.value.replace(/\D/g,'')"  maxlength="11">
					</div>
				</li>
				<li class="item flex pdtb15 bordB1">
					<span class="ll">开户行：</span>
					<div class="rr">
						<input class="edit" type="text" value="" placeholder="请输入您的银行卡的开户行">
					</div>
				</li>
				<li class="item flex pdtb15 bordB1">
					<span class="ll">银行卡号：</span>
					<div class="rr">
						<input class="edit" type="number" value="" placeholder="请输入您的银行卡号">
					</div>
				</li>
			</ul>
		</div>
		
		
		<div class="submitbtn" style="margin-top: 100px;">
			<button class="btn" @click="Router.navigateTo({route:{path:'/pages/user_myBankCard/user_myBankCard'}})">确认</button>
		</div>
	</div>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				is_show:false,
				mainData: []
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res){
				    console.log('getMainData', res);
				    self.mainData.push.apply(self.mainData,res.info.data);		        
				};
				self.$apis.orderGet(postData, callback);
			}
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/userInfor.css";
	
</style>

