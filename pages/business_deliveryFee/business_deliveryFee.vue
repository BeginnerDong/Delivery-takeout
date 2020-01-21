<template>
	<div>
		
		<div class="myExtendTop white center">
			<div class="yuan pdb10">今天总共{{dayCount}}单</div>
			<div class="bigNum" style="font-size: 32px;">{{dayPrice}}<span class="fs12">元</span></div>
			
		</div>
		
		<div class="jifenTwoNum flexRowBetween boxShaow radius5">
			<div class="item">
				<p class="pdb5 flexCenter fs13 color2">开始时间<img class="arrow" src="../../static/images/icon2.png" ></p>
				
				<div class="num fs12 color6">
					<picker mode="date" :value='start' @change="changeStartTime">
						{{start==''?'请选择':start}} 
					</picker>
				</div>
			</div>
			<div class="item">
				<p class="pdb5 flexCenter fs13 color2">结束时间<img class="arrow" src="../../static/images/icon2.png" ></p>
				<div class="num color6 fs13">
					<picker mode="date" :value='end' @change="changeEndTime">
						 {{end==''?'请选择':end}} 
					</picker>
				</div>
			</div>
		</div>
		
		<div class="orderList pdlr4">
			<ul>
				<li class="bordB1" v-for="(item,index) in mainData" :key="index">
					<div class="infor">
						<div class="datt flexRowBetween">
							<div class="left color6">订单编号：{{item.order_no}}</div>
							<div class="price">{{item.itemCount}}</div>
						</div>
						
						<div class="msg mglr10 pr radius5 fs12" v-if="item.type==5">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">{{item.title}}</span>
								<span class="num">×{{item.count}}</span>
								<span class="mny">￥{{item.price}}</span>
							</p>
						</div>
						<div class="msg mglr10 pr radius5 fs12" v-if="item.type==6">
							<p class="child  flexRowBetween" v-for="c_item in item.child[0]">
								<span class="tit avoidOverflow">{{c_item.title}}</span>
								<span class="num">×{{c_item.count}}</span>
								<span class="mny">￥{{c_item.unit_price}}</span>
							</p>
						</div>
					</div>
				</li>
			</ul>
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
				dlvrFreeDate:[{},{},{}],
				searchItem:{
					level:0,
					thirdapp_id:3,
					user_type:0,
					pay_status:1
				},
				dayCount:0,
				dayPrice:0,
				end:'',
				start:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			var day2 = new Date();
			day2.setTime(day2.getTime());
			var s2 = day2.getFullYear()+"-" + (day2.getMonth()+1) + "-" + day2.getDate();
			//明天的时间
			var day3 = new Date();
			day3.setTime(day3.getTime()+24*60*60*1000);
			var s3 = day3.getFullYear()+"-" + (day3.getMonth()+1) + "-" + day3.getDate();
			self.start = s2;
			self.end = s3;
			//self.$Utils.loadAll(['getMainData'], self);
			var dayStart = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
			var nowTime = (new Date()).getTime() / 1000;
			self.searchItem.create_time=['between',[dayStart,nowTime]];
		},
		
		onShow() {
			const self = this;
			self.mainData = [];
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeStartTime(e){
				const self = this;
				self.start = e.detail.value;
				
				self.startTimestamp = self.$Utils.timeToTimestamp(self.start);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
			changeEndTime(e){
				const self = this;
				self.end = e.detail.value;
				
				self.endTimestamp = self.$Utils.timeToTimestamp(self.end);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getStoreToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.compute = {  
				  rider_income:[
					'sum',
					'rider_income',
					{
					  status:1,level:0,thirdapp_id:3,user_type:0
					}
				  ],
				  platform_income:[
						'sum',
						'platform_income',
						{
						  status:1,level:0,thirdapp_id:3,user_type:0
						}
				  ]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						self.dayPrice = res.info.compute.platform_income + res.info.compute.rider_income;
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].itemCount = parseFloat(self.mainData[i].platform_income)+parseFloat(self.mainData[i].rider_income)
						}
					};
					self.dayCount = res.info.total;
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/user.css";
	@import "../../assets/style/order.css";
	
	.orderList li{box-shadow: initial; margin-top: 0;}
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4% 60px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; line-height:40px;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 15px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .right{width: 50%;}
</style>

