<template>
	<div>
		
		<div class="orderList f5bj" style="padding: 15px 4%;">
			<ul>
				<li style="margin-top: 0;">
					<div class="infor">
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
				</li>
				
			</ul>
		</div>
		
		<div class="pdlr4">
			<div class="line40">退款原因</div>
			<div class="pd10 f5bj radius5">
				<textarea disabled="disabled" auto-height class="w fs12 color6" style="line-height: 24px;border: none;display: block;" placeholder="" value="退款原因退款原因退款原因" />
			</div>
			<div class="line40 mgt15">商家回复</div>
			<div class="pd10 f5bj radius5">
				<textarea  disabled="disabled" auto-height class="w fs12 color6" style="line-height: 24px;border: none;display: block;" placeholder="" value="退款原因退款原因退款原因退款原因退款原因退款原因退款原因" />
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
	@import "../../assets/style/order.css";
	
</style>

