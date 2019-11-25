<template>
	<div>
		<div class="userHead">
			<div class="left">
				<img class="photo" src="../../static/images/about-img.png" mode=""/>
				<div style="width: 70%;">昵称昵称昵称昵称</div>
			</div>
		</div>
		
		<div class="userBox2 boxShaow radius10">
			<div class="flexRowBetween tit">
				<div>我的订单</div>
				<a class="more flex" href="user_myOrder.html">查看<img class="arrowR" src="../../static/images/icon.png" ></a>
			</div>
			<div class="menu">
				<ul class="flexRowBetween">
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/user_myOrder/user_myOrder'}})">
						<img src="../../static/images/about-icon.png"/>
						<p>待配送</p>
					</li>
					<li class="child"  @click="Router.navigateTo({route:{path:'/pages/user_myOrder/user_myOrder'}})">
						<img src="../../static/images/about-icon1.png">
						<p>配送中</p>
					</li>
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/user_myOrder/user_myOrder'}})">
						<img src="../../static/images/about-icon2.png">
						<p>已完成</p>
					</li>
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/user_myOrder/user_myOrder'}})">
						<img src="../../static/images/about-icon3.png">
						<p>退款</p>
					</li>
				</ul>
			</div>
		</div>
		
		<div class="XlineNav mglr4">
			<div class="info" @click="Router.navigateTo({route:{path:'/pages/address/address'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/about-iocn4.png">
					<span class="tt">我的地址</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info"  @click="Router.navigateTo({route:{path:'/pages/user_myMoney/user_myMoney'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/about-iocn5.png">
					<span class="tt">我的收益</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info" @click="Router.navigateTo({route:{path:'/pages/business_login/business_login'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/about-iocn6.png">
					<span class="tt">商家入口</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
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
	@import "../../assets/style/user.css";
	
</style>

