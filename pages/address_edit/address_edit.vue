<template>
	<div>
		<div class="addressAdd fs13">
			<ul>
				<li>
					<div class="title">联系人：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入真实姓名" value="张尧">
					</div>
				</li>
				<li>
					<div class="title">&nbsp;</div>
					<div class="rightMsg flex sexSelt">
						<div class="flexRowBetween sexSelt" style="width: 100px;">
							<span class="flex" @click="change('1')"><image class="icon" :src="num==1?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'" />男</span>
							<span class="flex" @click="change('2')"><image class="icon" :src="num==2?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'" />女</span>
						</div>
					</div>
				</li>
				<li>
					<div class="title">手机号码：</div>
					<div class="rightMsg">
						<input type="number" placeholder="请输入手机号码"  maxlength="11" value="15689636398">
					</div>
				</li>
				<li>
					<div class="title">选择地区：</div>
					<div class="rightMsg flexRowBetween">
						陕西省西安市莲湖区<img class="arrowR" src="images/icon.png" alt="">
					</div>
				</li>
				<li>
					<div class="title">详细地址：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入详细地址" value="陕西省西安市莲湖区土门街道创新路70号">
					</div>
				</li>
			</ul>
		</div>
		
		<div class="submitbtn" style="margin-top:50px;">
			<button type="button" class="btn" @click="Router.navigateTo({route:{path:'/pages/address/address'}})">保存地址</button>
		</div>
		<div class="submitbtn" >
			<button type="button" class="btn" style="border:1px solid #2FA0ED;background: #fff; color: #2FA0ED;">删除地址</button>
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
				mainData: [],
				addressLis:[{},{},{}],
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

