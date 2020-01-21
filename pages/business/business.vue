<template>
	<div>
		<div class="userHead">
			<div class="left">
				<img class="photo" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""/>
				<div style="width: 70%;">{{mainData.info?mainData.info.name:''}}</div>
			</div>
		</div>
		
		<div class="userBox2 boxShaow radius10">
			<div class="flexRowBetween tit">
				<div>我的订单</div>
				<div class="more flex"  @click="Router.navigateTo({route:{path:'/pages/business_myOrder/business_myOrder'}})">查看<img class="arrowR" src="../../static/images/icon.png" ></div>
			</div>
			<div class="menu">
				<ul class="flexRowBetween">
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/business_myOrder/business_myOrder'}})">
						<img src="../../static/images/merchants-icon.png"/>
						<p>全部订单</p>
					</li>
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/business_myOrder/business_myOrder'}})">
						<img src="../../static/images/merchants-icon1.png">
						<p>待接单</p>
					</li>
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/business_myOrder/business_myOrder'}})">
						<img src="../../static/images/merchants-icon2.png">
						<p>待退款</p>
					</li>
					<li class="child" @click="Router.navigateTo({route:{path:'/pages/business_myOrder/business_myOrder'}})">
						<img src="../../static/images/merchants-icon3.png">
						<p>已完成</p>
					</li>
				</ul>
			</div>
		</div>
		
		<div class="XlineNav mglr4">
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_myMoney/business_myMoney'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon4.png">
					<span class="tt">收益管理</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_store/business_store'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon5.png">
					<span class="tt">店铺管理</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_releaseGoods/business_releaseGoods'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon6.png">
					<span class="tt">发布商品</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_goods/business_goods'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon7.png">
					<span class="tt">商品管理</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_deliveryFee/business_deliveryFee'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon8.png">
					<span class="tt">配送费用</span>
				</div>
				<div class="right">
					<img class="arrowR" src="../../static/images/icon.png" >
				</div>
			</div>
			<div class="info flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/business_cashOut/business_cashOut'}})">
				<div class="flex">
					<img class="icon" src="../../static/images/merchants-icon9.png">
					<span class="tt">提现审核</span>
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
				mainData: {}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
				};
				postData.tokenFuncName = 'getStoreToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
			},
			
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/user.css";

</style>

