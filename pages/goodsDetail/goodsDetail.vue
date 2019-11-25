<template>
	<div>
		
		<!-- banner -->
		<div class="detailxqBan oh">
			<img src="../../static/images/goods-img.png" alt="">
		</div>
		
		<div class="pdlr4 pdt10 pdb10">	
			<h1 class="ftn">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</h1>
			<p class="pdt5 flexRowBetween">
				<span class="price">56</span>
				<span class="addCar fs12 flexEnd color6"><img style="width: 15px; height: 15px; display: block;margin-right: 5px;" src="../../static/images/goods-icon.png" alt=""/>加入购物车</span>
			</p>
		</div>
		
		<div class="f5H10"></div>
		<div class="xqInfor">
			<div class="cont">
				<h2 class="ftn title pdb5">商品详情</h2>
				<p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
			</div>
		</div>
		<div class="f5H10"></div>
		
		<div class="pdlr4 pdt15">
			<h2 class="ftn title">商品详情</h2>
			<div class="detail_pj">
				<ul>
					<li class="bordB1 pdtb10" v-for="(item,index) in evaluateData" :key="index">
						<div class="flexRowBetween fs12">
							<div class="flex name color6 pdb5">
								<image class="photo" src="../../static/images/about-img.png" />
								<span class="flexRowBetween">昵称昵称昵称昵称昵称昵称</span>
							</div>
							<div class="time color9 flexEnd">2019.10.22 10:00</div>
						</div>
						<h2 class="ftn text fs13">内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</h2>
					</li>
				</ul>
			</div>
			
		</div>
		
		<div class="xqbotomBar" >
			<div class="threeNav center w flex">
				<div class="item L flexCenter fs12"  @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<div class="flexColumn">
						<div class="mgt5"><image class="L-icon" src="../../static/images/home-icon7.png" /></div>
						<div>我的</div>
					</div>
				</div>
				<div class="item M flexCenter">
					<div class="mgr10">
						<image class="CarIcon" src="../../static/images/home-icon8.png" mode=""/>
					</div>
					<div class="fs12 flexColumn">
						<div class="pr price" v-show="!priceShow" @click="carMxShow"><span class="pr">59<em class="car-num">1</em></span></div>
						<p>配送费￥10</p>
					</div>
				</div>
				<a class="item R flexCenter white" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">去结算</a>
			</div>
		</div>
		
		<!-- 弹框背景 -->
		<div class="black-bj" v-show="is_show"></div>
		
		<!-- 费用明细 -->
		<div class="qjAlertCont carMxShow" style="padding-bottom:60px;" v-show="is_priceShow">
			<div class="flexRowBetween pdlr4 line40 bordB1 fs12 color6 f5bj">
				<span>配送费10元</span>
				<span class="flex">
					<image class="arrowR mgr5" src="../../static/images/management-icon1.png" style="width: 14px; height: 14px; display: block;" />清空购物车
				</span>
			</div>
			<div class="pdlr4 list pdt10">
				<ul>
					<li class="flexRowBetween pdb15 fs13" v-for="(item,index) in moneyMxDate" :key='index'>
						<span class="L avoidOverflow">外婆资质萝卜干</span>
						<span class="M">￥56.22</span>
						<span class="R numBox flexEnd">
							<div class="">-</div>
							<div class="num">1</div>
							<div class="on">+</div>
						</span>
					</li>
				</ul>
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
				priceShow:false,
				moneyMxDate:[{},{},{},{},{},{},{},{},{}],
				is_priceShow:false,
				evaluateData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			carMxShow(){
				const self = this;
				self.is_show = !self.is_priceShow;
				self.is_priceShow = !self.is_priceShow
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
	@import "../../assets/style/numCar.css";
	@import "../../assets/style/prodctDetail.css";
	page{padding-bottom: 60px;}
</style>

