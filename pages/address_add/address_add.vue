<template>
	<div>
		
		<div class="addressAdd fs13">
			<ul>
				<li>
					<div class="title">联系人：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入真实姓名">
					</div>
				</li>
				<li>
					<div class="title">&nbsp;</div>
					<div class="rightMsg flex sexSelt">
						<div class="flexRowBetween sexSelt" style="width: 100px;">
							<span class="flex" @click="change('1')"><image class="icon" :src="num==1?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'" />男</span>
							<span class="flex" @click="change('2')"><image class="icon" :src="num==2?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'"/>女</span>
						</div>
					</div>
				</li>
				<li>
					<div class="title">手机号码：</div>
					<div class="rightMsg">
						<input type="number" placeholder="请输入手机号码"  maxlength="11">
					</div>
				</li>
				<li>
					<div class="title">选择地区：</div>
					<div class="rightMsg flexRowBetween color9">
						请选择<img class="arrowR" src="../../static/images/icon.png" alt="">
					</div>
				</li>
				<li>
					<div class="title">详细地址：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入详细地址">
					</div>
				</li>
			</ul>
		</div>
		
		<div class="submitbtn" style="margin-top:50px;">
			<a class="btn" @click="Router.navigateTo({route:{path:'/pages/address/address'}})">保存地址</a>
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
				num:1
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(num){
				const self = this;	
				if(num!=self.num){
					self.num = num
				}
			},
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
	@import "../../assets/style/address.css";

</style>

