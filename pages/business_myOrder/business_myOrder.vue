<template>
	<div>
		
		<div class="orderNav">
			<div class="tt" :class="current==1?'on':''" @click="change('1')">全部订单</div>
			<div class="tt" :class="current==2?'on':''" @click="change('2')">待接单</div>
			<div class="tt" :class="current==3?'on':''" @click="change('3')">待退款</div>
			<div class="tt" :class="current==4?'on':''" @click="change('4')">已完成</div>
		</div>
		
		<div class="orderList pdlr4" style="margin-top: 60px;">
			<ul>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_Detail/business_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">待接单</div>
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
						<span class="Bbtn" @click="deltAlert">接单</span>
					</div>
				</li>
				<li>
					<div class="infor"  @click="Router.navigateTo({route:{path:'/pages/business_myOrder_Detail/business_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">待退款</div>
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
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_refundDetail/business_myOrder_refundDetail'}})">查看原因</span>
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_refuse/business_myOrder_refuse'}})">拒绝</span>
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_refuseOk/business_myOrder_refuseOk'}})">确定</span>
					</div>
				</li>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_Detail/business_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">已退款</div>
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
						<div class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_refundDetail/business_myOrder_refundDetail'}})">查看原因</div>
					</div>
				</li>
				<li>
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_Detail/business_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">已配送</div>
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
					<div class="infor" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_Detail/business_myOrder_Detail'}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：123356885555</div>
							<div class="state">已评价</div>
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
						<div class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/business_myOrder_evaluate/business_myOrder_evaluate'}})">查看评价</div>
					</div>
				</li>
			</ul>
		</div>
		
		
		<!-- 确认接单弹框 -->
		<div class="xieyiAlert" v-if="is_show" style="background: none;">
			<div class="infor center white" style="width:75%; padding:40px 30px;height: auto;border-radius: 10px;background: rgba(0,0,0,0.5);">
				<div class="colseBtn"  @click="deltAlert" style="top: 10px;right: 10px;left:auto;">×</div>
				<div class="tit pdb30  white">确定接单吗？</div>
				<div class="btnB flexRowBetween fs12">
					<div @click="deltAlert">取消</div>
					<div class="on">确定</div>
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
			deltAlert(){
				const self = this;
				self.is_show=!self.is_show;
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
	@import "../../assets/style/agreeSel.css";
	page{background: #F5F5F5;}
	.orderNav{ position: fixed;top: 50px;left: 0; width: 100%;box-sizing: border-box;background: #fff; z-index: 5;border-bottom: 1px solid #eee;}
</style>

