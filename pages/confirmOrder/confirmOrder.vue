<template>
	<div>
		
		<div class="whiteBj pdlr4">
			<div class="confimInfor">
				<ul>
					
					<li class="flexRowBetween" v-if="addressData.city" @click="Router.navigateTo({route:{path:'/pages/address/address'}})">
						<div class="flex" style="width: 90%;">
							<img class="mgr5" style="width: 16px; height: 19px; display: block;" src="../../static/images/confirm-icon2.png" >
							<div>
								<p class="ftn">{{addressData.city+addressData.detail}}</p>
								<p class="flex fs12 color9">
									<span>{{addressData.name}}</span>
									<span class="mgl10 mgr20">{{addressData.country==1?'先生':'女士'}}</span>
									<span>{{addressData.phone}}</span>
								</p>
							</div>
						</div>
						<div class="flexEnd" style="width:10%"><img class="arrowR" src="../../static/images/icon.png"></div>
					</li>
					<li v-else>
						<div class="adrsaAddBtn flexCenter pubColor" 
						@click="Router.navigateTo({route:{path:'/pages/address_add/address_add'}})">
						<img style="width: 15px; height: 15px; display: block;margin-right: 10px;" 
						src="../../static/images/confirm-icon.png" >新增收货地址
						</div>
					</li>
					<li class="flexRowBetween">
						<div class="flex">
							<img style="width: 14px; height: 14px; display: block;" src="../../static/images/confirm-icon1.png">
							
							<hTimePicker sTime="8" cTime="20" interval="15" @changeTime="changeTime">
							  <view slot="pCon" class="changeTime">
							    {{submitData.start_time}}
							  </view>
							</hTimePicker>
							<span class="pubColor fs12" v-if="distance>0">大约{{time}}分钟后送达</span>
							<span class="pubColor fs12" v-else>暂无法估算送达时间</span>
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
					<li class="flex onePro" style="align-items: initial;" v-for="item in cartData">
						<div class="pic mgr10">
							<img :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" >
						</div>
						<div class="proR pr">
							<h1 class="ftn flexRowBetween">
								<span class="avoidOverflow2" style="width:70%;">{{item.title}}</span>
								<span class="price fs13">{{item.price}}</span>
							</h1>
							<p class="numb color9 fs12">×{{item.count}}</p>
						</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">包装费</div>
						<div class="rr flexEnd price">{{submitData.packing_price}}</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">配送费</div>
						<div class="rr flexEnd price">{{deliverFee}}</div>
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
						<div class="rr flexEnd color9 fs13" @click="couponShow" v-if="couponData.length==0">
							暂无优惠券使用
							<img class="arrowR" src="../../static/images/icon.png" >
						</div>
						<div class="rr flexEnd color9 fs13" @click="couponShow" v-if="couponData.length>0&&chooseCoupon.length==0">
							请选择
							<img class="arrowR" src="../../static/images/icon.png" >
						</div>
						<div class="rr flexEnd color9 fs13" @click="couponShow" v-if="couponData.length>0&&chooseCoupon.length>0">
							暂无优惠券使用
							<img class="arrowR" src="../../static/images/icon.png" >
						</div>
					</li>
					
					<li class="flexRowBetween">
						<div class="Lname">小费</div>
						<div class="rr flexEnd color9 fs13"  @click="tippingShow">
							{{submitData.gratuity?submitData.gratuity+'元':'请选择'}}<img class="arrowR" src="../../static/images/icon.png" ></div>
					</li>
					<li class="flexRowBetween" style="align-items: initial;">
						<div class="Lname">备注</div>
						<div class="rr flexEnd color9 fs13" v-if="!is_show">
							<textarea rows="" cols="" placeholder="输入备注信息" v-model="submitData.passage1"></textarea>
						</div>
						<div class="rr flexEnd color9 fs13" v-else>
							<text>{{submitData.passage1}}</text>
						</div>
					</li>
					<li class="flexRowBetween">
						<div class="Lname">餐具数量</div>
						<div class="rr flexEnd color9 fs13" @click="cjNumShow">
							{{submitData.tableware==''?'未选择':submitData.tableware}}<img class="arrowR" src="../../static/images/icon.png" ></div>
					</li>
				</ul>
			</div>
		</div>
		
		<!-- 底部信息 -->
		<cover-view class="qusUnderFix flexEnd" style="z-index: 999;">
			<cover-view class="left flexEnd " @click="moneyMxShow">
				合计&nbsp;<cover-view class="" style="color: #ff3b3b">¥{{pay.wxPay?pay.wxPay.price:'0.00'}}</cover-view>
			</cover-view>
			<cover-view class="right" style="z-index: 100;"  @click="Utils.stopMultiClick(submit)">提交订单</cover-view>
		</cover-view>
		
		<!-- 弹框背景 -->
		<div class="black-bj" v-show="is_show"></div>
		
	
		
		<div class="qjAlertCont couponShow" v-show="is_couponShow">
			<div class="q-close" @click="couponShow">取消</div>
			<h1 class="center line40 bordB1 ftn">优惠券</h1>
			<div class="infor">
				<ul>
					<li v-for="(item,index) in couponData" :key="index"  @click="useCoupon(index)">
						<span>{{item.type==1?'抵扣券':'折扣券'}}{{item.type==2?item.discount/10+'折':item.value+'元'}}</span>
						<img class="setIcon" 
						:src="index==couponIndex?'../../static/images/coupons-icon.png':'../../static/images/coupons-icon1.png'" alt="">
					</li>
				</ul>
			</div>
		</div>
		
		<!-- 配送费弹框 -->
		<div class="qjAlertCont tippingShow" v-show="is_tippingShow">
			<div class="flexRowBetween pdlr4 line40 bordB1">
				<span class="fs12 color6" @click="tippingShow">取消</span>
				<span>小费</span>
				<span class="pucolor fs12" @click="confirmTip()">确定</span>
			</div>
			<div class="wpMsgBox pdlr4 mgt20">
				<div class="item flexRowBetween">
					<span v-for="(item,index) in tipDate" :key="index" :class="seltCurr == index?'on':''" 
					@click="seltSpecs(index)">{{item}}</span>
					<span><input placeholder="其他金额" v-model="tip" @focus="tipInput()"/></span>
				</div>
			</div>
		</div>
		
		<!-- 选择餐具数量弹框 -->
		<div class="qjAlertCont" v-show="is_cjNumShow">
			<div class="flexRowBetween pdlr4 line40 bordB1">
				<span class="fs12 color6" @click="cjNumShow">取消</span>
				<span>选择餐具数量</span>
				<span class="pucolor fs12" @click="confirmCj()">确定</span>
			</div>
			<div class="cjNumShow pdlr4 mgt20 flexColumn fs13 center pdb30">
				<span class="item" v-for="(item,index) in cjNumDate" :key="index" :class="cjNumCurr == index?'on':''" @click="seltCjNum(index)">{{item}}</span>
			</div>
		</div>
		
		<!-- 费用明细 -->
		<div class="qjAlertCont moneyMxShow" v-show="is_moneyMxShow" style="padding-bottom:60px;">
			<div class="flexRowBetween pdlr4 line40 bordB1">
				<span class="fs12 color6" @click="moneyMxShow">关闭</span>
				<span>费用明细</span>
				<!-- <span class="flex">
					<a class="pucolor fs12"  @click="Router.navigateTo({route:{path:'/pages/price_specs/price_specs'}})">价格规格</a>
					<img class="arrowR" src="../../static/images/icon.png" >
				</span> -->
			</div>
			<div class="pdlr4 list">
				<ul>
					<li class="flexRowBetween" v-for="(item,index) in moneyMxDate" :key='index'>
						<span>{{item.title}}<em class="fs12 color9">{{item.range}}</em></span>
						<span>{{item.price}}</span>
					</li>
				</ul>
			</div>
		</div>
		
	</div>
</template>


<script>
	import hTimePicker from "@/components/h-timePicker/h-timePicker.vue";
	export default {
		components: { hTimePicker },
		
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				is_show:false,
				mainData: [],
				is_couponShow:false,
				is_tippingShow:false,
				 seltCur:0,
				seltCurr:-1,
				tipCurr:0,
				tipDate:['5元','10元','15元','20元','25元'],
				cjNumCurr:0,
				is_cjNumShow:false,
				cjNumDate:['无需餐具','1份','2份','3份','4份'],
				seltTimeCurr:0,
				is_seltTimeShow:false,
				seltTimeDate:[{},{},{},{},{}],
				addressData:{},
				storeAddress:{},
				cartData:[],
				pay:{
					coupon:[]
				},
				distance:0,
				totalPrice:0,
				bjFee:0,
				tip:0,
				couponData:[],
				couponIndex:-1,
				chooseCoupon:[],
				time:'',
				submitData:{
					tableware:'',
					passage1:'',//备注
					start_time:'立即送出',//取件时间,
					main_price:0,
					packing_price:0,
					distance_price:0,
					weight_price:0,
					night_price:0,
					gratuity:0,
					insured_price:0,
					weather_price:0,
					holiday_price:0,
					rush_price:0,
					coupon_reduce:0,
					rider_income:0,
					platform_income:0,
					start_site:'',
					start_longitude:'',
					start_latitude:'',
					end_site:'',
					end_longitude:'',
					end_latitude:'',
					end_name:'',
					end_phone:'',
					city_id:'',
					invalid_time:'',
					price:0,
					delivery_reduce:0,
					snap_address:{},
					total_distance:''
				},
				packing_price:0,
				productPrice:0,
				deliverFee:0,
				is_moneyMxShow:false,
				moneyMxDate:[]
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
			self.getMainData()
		},
		
		onShow() {
			const self = this;
			var now = Date.parse(new Date())/1000;
			self.submitData.invalid_time = now+3600;
			self.cartData = self.$Utils.getStorageArray('cartDataTwo');
			self.submitData.city_id = uni.getStorageSync('user_info').thirdApp.city_id;
			if(uni.getStorageSync('storeAddress')){
				self.storeAddress = uni.getStorageSync('storeAddress');
				self.submitData.start_site = self.storeAddress.address;
				self.submitData.start_latitude = self.storeAddress.latitude;
				self.submitData.start_longitude = self.storeAddress.longitude;
			}
			
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData');
				self.submitData.end_site = self.addressData.city+self.addressData.detail;
				self.submitData.end_latitude = self.addressData.latitude;
				self.submitData.end_longitude = self.addressData.longitude;
				self.submitData.end_name = self.addressData.name;
				self.submitData.end_phone = self.addressData.phone;
				if(self.storeAddress.latitude&&self.addressData.latitude){
					self.getBaiduDistance()
				}
			}else{
				self.getAddressData()
				
			};
			self.productPrice = 0;
			self.packing_price = 0;
			self.submitData.main_price = 0;
			self.submitData.packing_price = 0;
			for (var i = 0; i < self.cartData.length; i++) {
				self.productPrice += self.cartData[i].price*self.cartData[i].count;
				self.packing_price += self.cartData[i].packing_price*self.cartData[i].count
			};
			console.log('productPrice',self.productPrice);
			console.log('packing_price',self.totalPrice);
			
			self.submitData.main_price = self.productPrice.toFixed(2);
			self.submitData.packing_price = self.packing_price.toFixed(2);
			
			
			console.log('self.cartData',self.cartData);
			console.log('self.totalPrice',self.totalPrice);
			self.getUserCouponData(true)
		},
		
		methods: {
			
			getBaiduDistance() {
				const self = this;
				const postData = {
					tokenFuncName:'getProjectToken',
					start_longitude:parseFloat(self.storeAddress.longitude),
					start_latitude:parseFloat(self.storeAddress.latitude),
					end_longitude:parseFloat(self.addressData.longitude),
					end_latitude:parseFloat(self.addressData.latitude)
				};
				const callback = (res) => {
					if(res.solely_code==100000){
						self.distance = parseFloat(res.info.distance/1000).toFixed(2);
						self.submitData.total_distance = self.distance;
						self.time = parseInt(self.distance*uni.getStorageSync('user_info').thirdApp.custom_rule.time);
						self.getMainData()
					}else{
						self.$Utils.showToast(res.msg, 'none');
					}
				};
				self.$apis.getBaiduDistance(postData, callback);
			},
			
			thirdAppGet() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.thirdAppData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.countPrice()
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.thirdAppGet(postData, callback);
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(JSON.stringify(self.addressData)=='{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择收获地址', 'none')
					return
				};
				if(parseFloat(self.distance)>parseFloat(self.thirdAppData.custom_rule.limit_distance)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('超出最远配送距离', 'none')
					return
				}
				var data = {};
				if(self.cartData.length>1){
					data.level = 1
				};
				var orderList = [
				
				];
				for (var i = 0; i < self.cartData.length; i++) {
					orderList.push(
						{product_id:self.cartData[i].id,count:self.cartData[i].count,type:5,data:data},
					)
				}
				console.log(self.submitData)
				//return
				self.addOrder(orderList)	
			},
			
			addOrder(orderList) {
				const self = this;	
				if(self.orderId){
					 self.payNow();
					 return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.data.snap_address = self.addressData;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.payNow()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			
			
			payNow(order_id) {
				const self = this;	
				
				const postData = self.$Utils.cloneForm(self.pay);
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user_myOrder/user_myOrder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user_myOrder/user_myOrder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			getUserCouponData(isNew) {
				const self = this;
				if(isNew){
					self.couponData = []
				};
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					type: ['in', [1, 2]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					
					self.$Utils.finishFunc('getUserCouponData');
			
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
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
			
			// 配送费弹框
			
			seltSpecs(index){
				const self = this;
				self.seltCurr=index;
				self.tip = parseInt(self.tipDate[self.seltCurr]);
				console.log('self.tip',self.tip)
			},
			
			tippingShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_tippingShow = !self.is_tippingShow
			},
			
			confirmTip(){
				const self = this;
				self.tippingShow();
				self.countPrice()
			},
			
			tipInput(event){
				const self = this;
				self.seltCurr = -1;
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				var d = new Date();
				var currHours=d.getHours();
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					param1:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							
							distance:['<',self.distance],
							type:5,
							fee_type:1
						},
						condition:'=',
						order:{
							distance:'desc'
						}
					},
					param2:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:2,
							is_use:1,
							start:['<=',currHours],
							end:['>=',currHours]
						},
						condition:'=',
					},
					param3:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:3,
							is_use:1
						},
						condition:'=',
					},
					param4:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:4,
							is_use:1
						},
						condition:'=',
					},
					param5:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:5,
							is_use:1
						},
						condition:'=',
					},
					param6:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:6,
							is_use:1,
						},
						condition:'=',
						order:{
							standard:'desc'
						}
					},
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('看这个',self.mainData)
					self.thirdAppGet();
					
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			useCoupon(index) {
				const self = this;	
				console.log(index)
				var id = self.couponData[index].id;
				self.couponIndex = -1;
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.is_couponShow = false;
				self.is_show = false;
				if(self.pay.coupon.length>=1){
					self.pay.coupon = []
					self.chooseCoupon = []
				};
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.pay.coupon.splice(findItem[0], 1);
					self.chooseCoupon = []
				} else {
					console.log('self.data.price - self.data.couponTotalPrice',self.totalPrice - self.couponTotalPrice);
					console.log('findCoupon.condition',findCoupon.condition);
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					};
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					};
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
					self.is_couponShow = false;
					self.is_show = false;
					self.couponIndex = index;
				};
				self.countPrice();
			},
			
			countPrice(){
				const self = this;
				
				var nowTime = Date.parse(new Date());
				self.totalPrice = 0;
				self.shopPrice = 0;
				self.submitData.distance_price=0;
				
				self.submitData.night_price=0;
				self.submitData.gratuity=0;
				self.submitData.insured_price=0;
				self.submitData.weather_price=0;
				self.submitData.holiday_price=0;
				self.submitData.rush_price=0;
				
				self.submitData.coupon_reduce=0;
				self.submitData.rider_income=0;
				self.submitData.platform_income=0;
			
				self.moneyMxDate = [];
				self.shopPrice = parseFloat(self.productPrice) + parseFloat(self.packing_price);
				self.moneyMxDate.push({title:'商品费用',price:'￥'+self.submitData.main_price});
				self.moneyMxDate.push({title:'包装费',price:'￥'+self.submitData.packing_price});
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				console.log(3242333333333)
				if(self.mainData.param6.length>0){
					for (var i = 0; i < self.mainData.param6.length; i++) {
						if(parseFloat(self.shopPrice)>parseFloat(self.mainData.param6[i].standard)){
							var index  = i
							break
						}
					};
					console.log(index)
					if(index||index==0){
						self.shopPrice = parseFloat(self.shopPrice)-parseFloat(self.mainData.param6[index].value);
						self.moneyMxDate.push({title:'满减',price:'-￥'+parseFloat(self.mainData.param6[index].value)})
					}
				};
				console.log('self.shopPrice',self.shopPrice)
				if(self.mainData&&self.mainData.param1&&self.mainData.param1.length>0){
					for (var i = 0; i < self.mainData.param1.length; i++) {
						if(i==0){
							self.submitData.distance_price = (self.distance-self.mainData.param1[i].distance)*self.mainData.param1[i].price
						}else{
							self.submitData.distance_price = (self.mainData.param1[i-1].distance-self.mainData.param1[i].distance)*self.mainData.param1[i].price+ self.submitData.distance_price
						}
						
					};
					
					self.submitData.distance_price = parseFloat(self.submitData.distance_price).toFixed(2);
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.submitData.distance_price)).toFixed(2);
					self.moneyMxDate.push({title:'距离附加',range:self.distance+'公里',price:'￥'+self.submitData.distance_price})
				};
				
				if(self.mainData.param2.length>0){
					
					self.submitData.night_price = parseFloat(self.mainData.param2[0].price).toFixed(2);
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.mainData.param2[0].price)).toFixed(2);
					self.moneyMxDate.push({title:'夜间配送费',price:'￥'+self.submitData.night_price})
				};
				var money =  (parseFloat(self.submitData.distance_price) + parseFloat(self.submitData.night_price)).toFixed(2);
				
			
				if(self.mainData.param3.length>0){
					
					self.submitData.weather_price = (money*(self.mainData.param3[0].rate/100)).toFixed(2);
					console.log('self.submitData.weather_price',self.submitData.weather_price);
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.submitData.weather_price)).toFixed(2);
					self.moneyMxDate.push({title:'恶劣天气附加费',price:'￥'+self.submitData.weather_price})
				};
				if(self.mainData.param4.length>0){
					
					self.submitData.holiday_price = (money*(self.mainData.param4[0].rate/100)).toFixed(2);
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.submitData.holiday_price)).toFixed(2);
					self.moneyMxDate.push({title:'节假日附加费',price:'￥'+self.submitData.holiday_price})
				};
				if(self.mainData.param5.length>0){
					
					self.submitData.rush_price = (money*(self.mainData.param5[0].rate/100)).toFixed(2);
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.submitData.rush_price)).toFixed(2);
					self.moneyMxDate.push({title:'高峰时段附加费',price:'￥'+self.submitData.rush_price})
				};
				
				if(self.tip>0){
					self.submitData.gratuity = self.tip;
					self.totalPrice = (parseFloat(self.totalPrice) +  parseFloat(self.submitData.gratuity)).toFixed(2);
					self.moneyMxDate.push({title:'小费',price:'￥'+self.submitData.gratuity})
				};
				
				if(self.couponTotalPrice>0){
					self.submitData.coupon_reduce = self.couponTotalPrice;
					
					self.moneyMxDate.push({title:'优惠券抵扣',price:'￥'+self.submitData.coupon_reduce})
				};
				
				self.submitData.platform_income = 
				((parseFloat(self.totalPrice))*
				(parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.tax)/100)).toFixed(2);
				
				self.deliverFee = parseFloat(self.totalPrice).toFixed(2);
				
				if((parseFloat(self.deliverFee)-parseFloat(self.thirdAppData.custom_rule.delivery_reduce))<0){
					
					var hasReduce = 0;
					hasReduce = parseFloat(self.deliverFee)
					self.submitData.delivery_reduce = parseFloat(self.deliverFee);
				}else{
					self.submitData.delivery_reduce = parseFloat(self.thirdAppData.custom_rule.delivery_reduce)
					var hasReduce = 0;
					hasReduce = parseFloat(self.thirdAppData.custom_rule.delivery_reduce)
				}
				if(parseFloat(hasReduce)>0){
					self.moneyMxDate.push({title:'配送费减免',price:'-￥'+parseFloat(self.submitData.delivery_reduce)})
				};
				self.deliverFee = (parseFloat(self.totalPrice)-parseFloat(self.submitData.delivery_reduce)).toFixed(2);
				console.log('11',parseFloat(self.totalPrice))
				console.log('22',parseFloat(self.shopPrice))
				console.log('22',parseFloat(self.couponTotalPrice))
				console.log('22',parseFloat(hasReduce))
				
				var wxPay = parseFloat(self.totalPrice) + parseFloat(self.shopPrice) - parseFloat(self.couponTotalPrice) - parseFloat(hasReduce);
				
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay.toFixed(2),	
					};
					
				} else {
					  delete self.pay.wxPay;
					  wxPay = 0
				};
				if(parseFloat(hasReduce)>0){
					self.pay.other = {
						price: parseFloat(hasReduce),	
					}
				};
				
				self.submitData.rider_income = (parseFloat(self.totalPrice) - parseFloat(self.submitData.platform_income)).toFixed(2);
				self.submitData.price = wxPay.toFixed(2);
	
				console.log('self.moneyMxDate',self.moneyMxDate)
				
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
			
			confirmCj(){
				const self = this;
				self.submitData.tableware = self.cjNumDate[self.cjNumCurr];
				self.is_show = !self.is_show
				self.is_cjNumShow = !self.is_cjNumShow
			},
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
						self.submitData.end_site = self.addressData.city+self.addressData.detail;
						self.submitData.end_latitude = self.addressData.latitude;
						self.submitData.end_longitude = self.addressData.longitude;
						self.submitData.end_name = self.addressData.name;
						self.submitData.end_phone = self.addressData.phone;
						if(self.storeAddress.latitude&&self.addressData.latitude){
							self.getBaiduDistance()
						};
						console.log('self.distance',self.distance)
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			// 价格明细
			moneyMxShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_moneyMxShow = !self.is_moneyMxShow
			},	
			
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/confirmOrder.css";
	page{background: #F5F5F5; padding-bottom: 60px;}
	/* 费用明细 */
	.moneyMxShow .list li{padding: 10px 0; font-size: 13px;}
	.moneyMxShow .list li em{margin-left: 5px;}
</style>

