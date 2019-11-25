<template>
	<div>
		
		<div class="orderNav">
			<div class="tt" :class="current==1?'on':''" @click="change('1')">全部订单</div>
			<div class="tt" :class="current==2?'on':''" @click="change('2')">待配送</div>
			<div class="tt" :class="current==3?'on':''" @click="change('3')">配送中</div>
			<div class="tt" :class="current==4?'on':''" @click="change('4')">已完成</div>
			<div class="tt" :class="current==5?'on':''" @click="change('5')">退款</div>
		</div>
		
		<div class="orderList pdlr4" style="margin-top: 60px;">
			<ul>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">待配送</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
					<div class="underBtn pdlr10 mgt15">
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_refund/user_myOrder_refund'}})">退款</span>
					</div>
				</li>
				<li>
					<div class="infor"  @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">配送中</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
				</li>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">已完成</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
					<div class="underBtn pdlr10 mgt15">
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_evaluate/user_myOrder_evaluate'}})">去评价</span>
					</div>
				</li>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">退款中</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
					<div class="underBtn pdlr10 mgt15">
						<span class="Bbtn"  @click="Router.navigateTo({route:{path:'/pages/user_myOrder_refundDetail/user_myOrder_refundDetail'}})">查看详情</span>
					</div>
				</li>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">退款完成</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
					<div class="underBtn pdlr10 mgt15">
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user_myOrder_refundDetail_Reply/user_myOrder_refundDetail_Reply'}})">查看详情</span>
					</div>
				</li>
			</ul>
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
				current:1
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.orderNav{ position: fixed;top: 50px;left: 0; width: 100%;box-sizing: border-box;background: #fff; z-index: 5;border-bottom: 1px solid #eee;}

</style>

