<template>
	<div>
		
		<div class="whiteBj pdlr4">
			<div class="confimInfor">
				<ul>
					<li>
						<div class="adrsaAddBtn flexCenter pubColor" @click="Router.navigateTo({route:{path:'/pages/address_add/address_add'}})"><img style="width: 15px; height: 15px; display: block;margin-right: 10px;" src="../../static/images/confirm-icon.png" >新增收货地址</div>
					</li>
					<li class="flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/address/address'}})">
						<div class="flex" style="width: 90%;">
							<img class="mgr5" style="width: 16px; height: 19px; display: block;" src="../../static/images/confirm-icon2.png" >
							<div>
								<p class="ftn">陕西省西安市莲湖区土门街道创新路70号</p>
								<p class="flex fs12 color9">
									<span>张尧</span>
									<span class="mgl10 mgr20">男士</span>
									<span>15689636398</span>
								</p>
							</div>
						</div>
						<div class="flexEnd" style="width:10%"><img class="arrowR" src="../../static/images/icon.png"></div>
					</li>
					<li class="flexRowBetween">
						<div class="flex">
							<img style="width: 14px; height: 14px; display: block;" src="../../static/images/confirm-icon1.png">
							<span class="mglr5">立即送出</span>
							<span class="pubColor fs12">大约14:48送达</span>
						</div>
						<div  @click="seltTimeShow">
							<img class="arrowR" src="../../static/images/icon.png" >
						</div>
					</li>
				</ul>
			</div>
		</div>
		
		<div class="mglr4">
			<div class="confimInfor pdlr10 whiteBj radius5 mgt15">
				<ul>
					<li class="flex onePro" style="align-items: initial;">
						<div class="pic mgr10">
							<img src="../../static/images/home-img.png" >
						</div>
						<div class="proR pr">
							<h1 class="ftn flexRowBetween">
								<span class="avoidOverflow2" style="width:70%;">蔬菜沙拉</span>
								<span class="price fs13">56</span>
							</h1>
							<p class="numb color9 fs12">×1</p>
						</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">包装费</div>
						<div class="rr flexEnd price">5.5</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">配送费</div>
						<div class="rr flexEnd price">3</div>
					</li>
				</ul>
			</div>
			
			<div class="confimInfor pdlr10 whiteBj radius5 mgt15">
				<ul>
					<li class="flexRowBetween">
						<div class="Lname">支付方式</div>
						<div class="rr flexEnd">在线支付</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">优惠券</div>
						<div class="rr flexEnd color9 fs13" @click="couponShow">请选择<img class="arrowR" src="../../static/images/icon.png" ></div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">配送消费</div>
						<div class="rr flexEnd color9 fs13"  @click="tippingShow">请选择<img class="arrowR" src="../../static/images/icon.png" ></div>
					</li>
					<li class="flexRowBetween" style="align-items: initial;">
						<div class="Lname">备注</div>
						<div class="rr flexEnd color9 fs13">
							<textarea rows="" cols="" placeholder="输入备注信息"></textarea>
						</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">餐具数量</div>
						<div class="rr flexEnd color9 fs13" @click="cjNumShow">未选择<img class="arrowR" src="../../static/images/icon.png" ></div>
					</li>
				</ul>
			</div>
		</div>
		
		<!-- 底部信息 -->
		<div class="qusUnderFix flexEnd">
			<div class="left flexEnd fs12">
				合计&nbsp;<em class="price">64.5</em>
			</div>
			<div class="right">提交订单</div>
		</div>
		
		<!-- 弹框背景 -->
		<div class="black-bj" v-show="is_show"></div>
		
		<!-- 时间选择 -->
		<div class="qjAlertCont seltTimeShow fs13 line24" v-show="is_seltTimeShow">
			<div class="flexRowBetween bordB1" style="align-items: initial;">
				<div class="left center pdt15">今天周四</div>
				<div class="right pdt10 pdb30">
					<div class="item flexRowBetween color6" :class="seltTimeCurr == index?'on':''"  v-for="(item,index) in seltTimeDate" :key="index"  @click="seltTime(index)">
						<span class="L">立即送出</span>
						<span class="M">3.0元配送费</span>
					</div>
				</div>
			</div>
			<div class="w line40 center fs13" @click="seltTimeShow">取消</div>
		</div>
		
		<!-- 优惠券 -->
		<div class="qjAlertCont couponShow" v-show="is_couponShow">
			<div class="q-close" @click="couponShow">取消</div>
			<h1 class="center line40 bordB1 ftn">优惠券</h1>
			<div class="infor">
				<ul>
					<li v-for="(item,index) in couponDate" @click="seltCoupon(index)">
						<span>{{item}}</span>
						<image class="setIcon" :src="seltCur == index?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'" alt="" />
					</li>
				</ul>
			</div>
		</div>
		
		<!-- 配送费弹框 -->
		<div class="qjAlertCont tippingShow" v-show="is_tippingShow">
			<div class="flexRowBetween pdlr4 line40 bordB1">
				<span class="fs12 color6" @click="tippingShow">取消</span>
				<span>小费</span>
				<span class="pucolor fs12">确定</span>
			</div>
			<div class="wpMsgBox pdlr4 mgt20">
				<div class="item flexRowBetween fs13">
					<span v-for="(item,index) in tipDate" :key="index" :class="tipCurr == index?'on':''" @click="seltSpecs(index)">{{item}}</span>
				</div>
			</div>
		</div>
		
		<!-- 选择餐具数量弹框 -->
		<div class="qjAlertCont" v-show="is_cjNumShow">
			<div class="flexRowBetween pdlr4 line40 bordB1">
				<span class="fs12 color6" @click="cjNumShow">取消</span>
				<span>选择餐具数量</span>
				<span class="pucolor fs12">确定</span>
			</div>
			<div class="cjNumShow pdlr4 mgt20 flexColumn fs13 center pdb30">
				<span class="item" v-for="(item,index) in cjNumDate" :key="index" :class="cjNumCurr == index?'on':''" @click="seltCjNum(index)">{{item}}</span>
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
				is_couponShow:false,
				seltCur:0,
				couponDate:['抵扣券5元','满20元抵3元'],
				tipCurr:0,
				tipDate:['不加了','￥5','￥10','￥15','￥20','￥25','其他金额'],
				cjNumCurr:0,
				is_cjNumShow:false,
				cjNumDate:['无需餐具','1份','2份','3份','4份'],
				seltTimeCurr:0,
				is_seltTimeShow:false,
				seltTimeDate:[{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			// 选择时间
			seltTimeShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_seltTimeShow = !self.is_seltTimeShow
			},
			seltTime(index){
				const self = this;
				self.seltTimeCurr = index
			},
			// 优惠券弹框
			couponShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_couponShow = !self.is_couponShow
			},
			seltCoupon(index){
				const self = this;
				self.seltCur = index
			},
			// 配送费弹框
			tippingShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_tippingShow = !self.is_tippingShow
			},
			seltSpecs(index){
				const self = this;
				self.tipCurr=index
			},
			
			// 选择餐具数量
			cjNumShow(index){
				const self = this;
				self.is_show = !self.is_show
				self.is_cjNumShow = !self.is_cjNumShow
			},
			seltCjNum(index){
				const self = this;
				self.cjNumCurr = index
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
	@import "../../assets/style/confirmOrder.css";
	page{background: #F5F5F5; padding-bottom: 60px;}
</style>

