<template>
	<div>
		<div class="myRowBetween" >
			<ul>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">提现金额</p>
					<p class="right flexEnd red fs13">562</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">姓名</p>
					<p class="right flexEnd">张丹</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">手机号</p>
					<p class="right fs13 flexEnd">18236562213</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">开户行</p>
					<p class="right fs13 flexEnd">中国银行</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">银行卡号</p>
					<p class="right fs13 flexEnd">6215562356235623</p>
				</li>
			</ul>
		</div>
		
		<div class="pdlr4 submitbtn flexRowBetween" style="margin-top: 100px;">
			<button class="btn" style="width: 43%;margin: 0 10px;background: #addaf9;" type="button">取消</button>
			<button class="btn" style="width: 43%;margin: 0 10px;" type="button">确定</button>
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
	
</style>

