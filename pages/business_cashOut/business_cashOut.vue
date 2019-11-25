<template>
	<div>
		<div class="orderNav">
			<div class="tt" :class="current==1?'on':''" @click="change('1')">提现审核</div>
			<div class="tt" :class="current==2?'on':''" @click="change('2')">提现流水</div>
		</div>
		
		<div class="pdlr4 bs-CashLis" style="margin-top: 55px;" v-show="current==1">
			<ul>
				<li class="radius5 boxShaow flexRowBetween" v-for="(item,index) in CashLisData" @click="Router.navigateTo({route:{path:'/pages/business_cashOutDetail/business_cashOutDetail'}})">
					<div class="ll">
						<p class="flex pdb10">
							<span class="tt color6">提现金额</span>
							<span class="red">592</span>
						</p>
						<p class="flex">
							<span class="tt color6">姓名</span>
							<span class="color2">张丹</span>
						</p>
					</div>
					<div class="rr">
						<a class="flexEnd pdtb20" >
							<img class="arrowR" src="../../static/images/icon.png" >
						</a>
					</div>
				</li>
			</ul>
		</div>
		
		<div class="pdlr4 bs-CashLis" style="margin-top: 55px;" v-show="current==2">
			<ul>
				<li class="radius5 boxShaow" v-for="(item,index) in CashLisData">
					<p class="flex pdb10">
						<span class="tt color6">提现金额</span>
						<span class="red">592</span>
					</p>
					<p class="flex pdb10">
						<span class="tt color6">姓名</span>
						<span class="color2">张丹</span>
					</p>
					<p class="flex">
						<span class="tt color6">时间</span>
						<span class="color2">2019/10/24</span>
					</p>
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
				current:1,
				CashLisData:[{},{},{}]
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
	@import "../../assets/style/agreeSel.css";
	
	page{background: #F5F5F5;padding-bottom: 30px}
	.orderNav{ position: fixed;top: 50px;left: 0; width: 100%;box-sizing: border-box;background: #fff; z-index: 5;border-bottom: 1px solid #eee;padding: 0 4%;}
	.orderNav .tt{width: 50%;}
	.orderNav .on::after{width: 50px; }
	.bs-CashLis li{padding: 15px; background: #fff; margin-bottom: 15px;}
	.bs-CashLis li .tt{width: 80px;}
	.bs-CashLis li .rr{width: 15%; display: block;}
</style>

