<template>
	<div>
		<div class="pdlr4">
			<!-- banner -->
			<div class="ind_banner oh pdtb15 radius10">
				<img src="../../static/images/home-banner.png" alt="">
			</div>
			<!-- banner end -->
			
			<div class="inde_msgBox">
				<ul>
					<li class="flex">
						<img class="Licon" src="../../static/images/home-icon2.png">
						<span class="fs14">肯德基宅急送</span>
						<span>00:00-24:00</span>
					</li>
					<li class="flexRowBetween">
						<div class="flex">
							<img class="Licon" src="../../static/images/home-icon1.png">
							<span class="color6 fs12">高新区高新大都荟</span>
						</div>
						<img class="RadrsIcon" src="../../static/images/home-icon3.png" >
					</li>
					<li class="flex">
						<img class="Licon" src="../../static/images/home-icon.png">
						<span class="color6 fs12">15689568975</span>
					</li>
				</ul>
			</div>
		</div>
		<div class="f5H5"></div>
		
		<div class="inde_prolis pr" style="min-height: 260px;">
			<div class="store_LeftNav center">
				<div class="item" :class="curr==index?'on':''" v-for="(item,index) in leftNavDate" :key="index" @click="chageNav(index)">{{item}}</div>
			</div>
			<div class="store_rightCont">
				<div class="shopOrder">
					<div class="item" v-for="(item,index) in proListDate" :key="index">
						<div class="infor">
							<div class="ll" @click="Router.navigateTo({route:{path:'/pages/goodsDetail/goodsDetail'}})">
								<img class="pic" src="../../static/images/home-img.png"/>
							</div>
							<div class="rr">
								<div class="titile fs13 avoidOverflow2" @click="Router.navigateTo({route:{path:'/pages/goodsDetail/goodsDetail'}})">标题标题标题标题标题标题标题标题</div>
								<div class="flexRowBetween Bmny">
									<div class="flex">
										<div class="price fs13">59</div>
										<div class="flex fs12 red mgl10 mgr10"><img class="zhe" src="../../static/images/home-icon4.png">8折</div>
										<div class="yuanJia fs12">69</div>
									</div>
									
									<div class="flexEnd">
										<div class="addBtn font14" v-show="item.addBtn" @click="addBtnShow">+</div>
										<div class="numBox flexEnd"  v-show="item.numBox">
											<div class="" @click="counter(index,'-')">-</div>
											<div class="num">{{item.count}}</div>
											<div class="on" @click="counter(index,'+')">+</div>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		
		<div class="xqbotomBar" >
			<div class="threeNav center w flex">
				<a class="item L flexCenter fs12" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<div class="flexColumn">
						<div class="mgt5"><img class="L-icon" src="../../static/images/home-icon7.png" ></div>
						<div>我的</div>
					</div>
				</a>
				<div class="item M flexCenter">
					<div class="mgr10">
						<img class="CarIcon" src="../../static/images/home-icon8.png" mode=""/>
					</div>
					<div class="fs12 flexColumn">
						<div class="pr price" v-show="priceShow" @click="carMxShow"><span class="pr">59<em class="car-num">1</em></span></div>
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
					<img class="arrowR mgr5" src="../../static/images/management-icon1.png" style="width: 14px; height: 14px; display: block;" >清空购物车
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
				curr:0,
				leftNavDate:['折扣','热销','套餐','饮品'],
				proListDate:[
					{count:'1',addBtn:true,numBox:false},
					{count:'1',addBtn:false,numBox:true},
					{count:'1',addBtn:false,numBox:true}
				],
				priceShow:false,
				moneyMxDate:[{},{},{},{},{},{},{},{},{}],
				is_priceShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			addBtnShow(){
				const self = this;
				self.priceShow = true
			},
			chageNav(index){
				const self = this;
				self.curr = index
			},
			carMxShow(){
				const self = this;
				self.is_show = !self.is_priceShow;
				self.is_priceShow = !self.is_priceShow
			},
			counter(index,type) {
				const self = this;
				
				if (type == '+') {
					self.proListDate[index].count++;
				} else {
					if (self.proListDate[index].count > 1) {
						self.proListDate[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.proListDate[index], 'id', 999);
				
				self.countTotalPrice();
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
	@import "../../assets/style/numCar.css";
	@import "../../assets/style/prodctDetail.css";

</style>

