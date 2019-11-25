<template>
	<div>
		<div class="storeGl">
			<ul>
				<li class="item flexRowBetween">
					<div class="ll title fs13">店铺名称</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="名称名称名称名称名称名称" placeholder="请输入店铺名称" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">商品价格</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="188" placeholder="请输入商品价格" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">折扣</div>
					<div class="rr flexEnd">
						<input type="number" name="" value="8折" placeholder="请输入商品折扣" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最终价格</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="56" placeholder="请输入店铺地址" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">包装费</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="10" placeholder="请输入包装费" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">库存</div>
					<div class="rr flexEnd fs12 color3">200</div>
				</li>
				
				<li class="item">
					<div class="title fs13">商品图片</div>
					<div class="flex uploadImg">
						<div class="lis pr">
							<img src="../../static/images/management-img.png" >
							<input class="upBtn" type="file" name="" value="" placeholder="" />
						</div>
					</div>
				</li>
				<li class="item">
					<div class="title fs13">商品详情图片</div>
					<div class="flex uploadImg">
						<div class="lis pr">
							<img src="../../static/images/management-img.png" >
							<input class="upBtn" type="file" name="" value="" placeholder="" />
						</div>
					</div>
				</li>
				<li class="item flexRowBetween" style="align-items: initial;">
					<div class="ll title fs13" style="width:20%">商品描述</div>
					<div class="rr flexEnd" style="width:80%">
						<textarea style="text-align: left;" rows="" cols="" placeholder="请输入商品描述" >商品描述内容商品描述内容商品描述内容商品描述内容商品描述内容商品描述内容商品描述内容</textarea>
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
	@import "../../assets/style/storeGl.css";

</style>

