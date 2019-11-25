<template>
	<div>
		
		<div class="pdlr4 mgt15">
			<div class="shopOrder">
				<div class="item radius5 boxShaow" v-for="(item,index) in proListDate" :key="index">
					<div class="infor">
						<div class="ll" href="goodsDetail.html">
							<img class="pic" src="../../static/images/home-img.png"/>
						</div>
						<div class="rr">
							<a class="titile fs13 avoidOverflow2" href="goodsDetail.html">标题标题标题标题标题标题标题标题</a>
							<div class="Bmny">
								<div class="flex">
									<div class="price fs13">59</div>
									<div class="flex fs12 red mgl10 mgr10"><img class="zhe" src="../../static/images/home-icon4.png">8折</div>
									<div class="yuanJia fs12">69</div>
								</div>
							</div>
						</div>
					</div>
					<div class="under_btn flexEnd pdt10 color6 fs12">
						<div class="btn flexEnd" @click="Router.navigateTo({route:{path:'/pages/business_goods_edit/business_goods_edit'}})"><img class="icon" src="../../static/images/management-icon.png">修改</div>
						<div class="btn flexEnd mgl40" @click="deltAlert"><img class="icon" src="../../static/images/management-icon.png" >删除</div>
						<div class="btn flexEnd mgl40" @click="deltShelf"><img class="icon" src="../../static/images/management-icon.png" >下架</div>
					</div>
				</div>
			</div>
		</div>
		
		
		<!-- 确定删除弹框 -->
		<div class="xieyiAlert" v-if="is_show">
			<div class="infor center radius5" style="padding:40px 30px;height: auto;">
				<div class="colseBtn"  @click="deltAlert" style="top: 10px;right: 10px;left:auto;">×</div>
				<div class="tit font16 pdb30">确定删除这个商品吗？</div>
				<div class="btnB flexRowBetween fs12">
					<div>取消</div>
					<div class="on">确定</div>
				</div>
			</div>
		</div>
		
		<!-- 确定删除下架商品弹框 -->
		<div class="xieyiAlert" v-if="is_Shelfshow">
			<div class="infor center radius5" style="padding:40px 30px;height: auto;">
				<div class="colseBtn"  @click="deltShelf" style="top: 10px;right: 10px;left:auto;">×</div>
				<div class="tit font16 pdb30">确定下架这个商品吗？</div>
				<div class="btnB flexRowBetween fs12">
					<div>取消</div>
					<div class="on" >确定</div>
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
				proListDate:[
					{count:'1',addBtn:true,numBox:false},
					{count:'1',addBtn:false,numBox:true},
					{count:'1',addBtn:false,numBox:true}
				],
				is_Shelfshow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			deltAlert(){
				const self = this;
				self.is_show=!self.is_show;
			},
			deltShelf(){
				const self = this;
				self.is_Shelfshow=!self.is_Shelfshow;
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
	@import "../../assets/style/index.css";
	@import "../../assets/style/agreeSel.css";
	
	
	.shopOrder{max-height: initial;  overflow-y: initial;}
	.shopOrder .item{padding: 15px 4%;}
	.shopOrder .item .infor{padding-left: 90px;}
	.shopOrder .item .ll{width: 80px; height: 80px;}
	.shopOrder .item .rr{height: 80px;}
	.under_btn .icon{width: 14px; height: 14px; display: block;margin-right: 5px;}
</style>

