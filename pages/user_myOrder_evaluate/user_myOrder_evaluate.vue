<template>
	<div>
		<div class="orderList f5bj" style="padding: 15px 4%;">
			<ul>
				<li style="margin-top: 0;">
					<div class="infor">
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
				</li>
				
			</ul>
		</div>
		
		<div class="pdlr4">
			<div class="line40">填写评价</div>
			<div>
				<textarea class="w radius5 fs12" style="height: 160px;box-sizing: border-box;border: none; background: #f8f8f8;padding: 15px; display: block;" rows="" cols="" placeholder="请写下您对这次交易的看法" value=""/>
			</div>
		</div>
		<div class="submitbtn" style="margin-top: 60px;">
			<button class="btn" type="submit">提交</button>
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

