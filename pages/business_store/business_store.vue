<template>
	<div>
		
		<div class="storeGl">
			<ul>
				<li class="item flexRowBetween">
					<div class="ll title fs13">店铺名称</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入店铺名称" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">营业时间</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入营业时间" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">联系电话</div>
					<div class="rr flexEnd">
						<input type="number" name="" value="" placeholder="请输入联系电话" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">店铺地址</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入店铺地址" />
					</div>
				</li>
				<li class="item">
					<div class="title fs13">店铺主图</div>
					<div class="flex uploadImg">
						<div class="lis pr">
							<img src="../../static/images/merchants-icon10.png" >
							<input class="upBtn" type="file" name="" value="" placeholder="" />
						</div>
						<div class="lis">
							<img src="../../static/images/management-img.png" >
						</div>
					</div>
					<p class="fs12 color9 pdt10">可传1~5张</p>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">愿意承担配送费用</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入承担配送费用" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最低下单金额</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入最低下单金额" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最远配送范围</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入最远配送范围" />
					</div>
				</li>
				<li class="item flexRowBetween" style="align-items: initial;">
					<div class="ll title fs13">设置满减</div>
					<div class="rr flexColumn manjian fs12 color9">
						<div class="w flexEnd">满<input type="text">元，减<input type="text">元</div>
						<div class="w flexEnd">满<input type="text">元，减<input type="text">元</div>
						<div class="w flexEnd">满<input type="text">元，减<input type="text">元</div>
					</div>
				</li>
				
			</ul>
		</div>
		
		<div class="submitbtn mgt40">
			<button class="btn" type="submit">确定</button>
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
	@import "../../assets/style/storeGl.css";
	
</style>

