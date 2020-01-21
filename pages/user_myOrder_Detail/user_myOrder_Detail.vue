<template>
	<div>
		<div class="pdlr4">
			<div class="pdtb10 bordB1">
				<div class="flex Toptit"><img class="icon" src="../../static/images/the_order_details-icon.png" >物品信息</div>
			</div>
			<div class="orederDetal">
				<ul>
					<li class="flexRowBetween fs13 msgLis" v-if="mainData.type==5">
						<p class="tit avoidOverflow">{{mainData.title}}</p>
						<p class="specs flexEnd">
							<span class="num">×{{mainData.count}}</span>
							<span class="mny">￥{{mainData.main_price}}</span>
						</p>
					</li>
					<li class="flexRowBetween fs13 msgLis" v-if="mainData.type==6" v-for="(item,index) in mainData.child[0]">
						<p class="tit avoidOverflow">{{item.title}}</p>
						<p class="specs flexEnd">
							<span class="num">×{{item.count}}</span>
							<span class="mny">￥{{item.price}}</span>
						</p>
					</li>
					<li class="flexRowBetween fs13 msgLis" >
						<p class="tit avoidOverflow">包装费</p>
						<p class="specs flexEnd">
							<!-- <span class="num">×{{item.count}}</span> -->
							<span class="mny">￥{{mainData.packing_price}}</span>
						</p>
					</li>
					<li class="flexEnd red">
						合计<span class="price">{{mainData.price}}</span>
					</li>
				</ul>
			</div>
		</div>
		<div class="f5H5"></div>
		<div class="pdlr4">
			<div class="pdtb10 bordB1">
				<div class="flex Toptit"><img class="icon" src="../../static/images/the_order_details-icon1.png">位置信息</div>
			</div>
			<div class="fs13 GprsMsg pdtb10">
				<div class="item flexRowBetween mgb10">
					<p class="adrs flex"><em class="dian" style="background: #009944;"></em>{{mainData.end_site}}</p>
					<!-- <span class="flexEnd"><img class="Ricon" src="../../static/images/the_order_details-icon7.png"></span> -->
				</div>
				<div class="item flexRowBetween">
					<p class="adrs flex"><em class="dian"></em>{{mainData.end_name}}&nbsp;{{mainData.end_phone}}</p>
					<!-- <span class="flexEnd"><img class="Ricon" src="../../static/images/the_order_details-icon8.png"></span> -->
				</div>
			</div>
		</div>
		<div class="f5H5" v-if="mainData.passage1!=''"></div>
		<div class="pdlr4" v-if="mainData.passage1!=''">
			<div class="pdtb10 bordB1">
				<div class="flex Toptit"><img class="icon" src="../../static/images/the_order_details-icon2.png">备注信息</div>
			</div>
			<div class="pdtb10 fs13 color6">{{mainData.passage1}}</div>
		</div>
		<div class="f5H5"></div>
		
		
		<div class="pdlr4">
			<div class="pdtb10 bordB1">
				<div class="flex Toptit"><img class="icon" src="../../static/images/the_order_details-icon3.png">派送时间</div>
			</div>
			<div class="pdtb10 flexRowBetween fs13">
				<span class="color6">派单时间：</span>
				<span>{{mainData.create_time}}</span>
			</div>
		</div>
		<div class="f5H5"></div>
		
		<div class="pdlr4" v-if="mainData.transport_status==2">
			<div class="pdtb10 bordB1">
				<div class="flex Toptit"><img class="icon" src="../../static/images/the_order_details-icon4.png">配送员信息</div>
			</div>
			<div class="flexRowBetween pdtb10 bordB1 fs13">
				<span>姓名：</span>
				<span>{{mainData.riderInfo&&mainData.riderInfo[0]?mainData.riderInfo[0].name:''}}</span>
			</div>
			<div class="flexRowBetween pdtb10 fs13">
				<span>电话：</span>
				<span>{{mainData.riderInfo&&mainData.riderInfo[0]?mainData.riderInfo[0].phone:''}}</span>
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
				 mainData: {},
				msgLisDate:[
					{title:'韩国泡菜',num:'1',price:'21.0'},
					{title:'韩国土豆饼',num:'1',price:'4.0'},
					{title:'韩国泡菜牛',num:'1',price:'21.0'},
					{title:'餐盒费',price:'1.0'}
				]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					riderInfo:{
						tableName:'UserInfo',
						middleKey:'rider_no',
						key:'user_no',
						searchItem:{
							status:1,
							thirdapp_id:2
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
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
	@import "../../assets/style/confirmOrder.css";
	page{padding-bottom: 50px;}
	.Toptit .icon{ width:15px; height: 15px; display: block;margin-right: 10px;}
	.orederDetal li{padding: 10px 0; border-bottom:1px solid #eee;}	
	.orederDetal li:last-child{border-bottom: 0;}
	.orederDetal .msgLis .tit{width:60%; color: #666;}
	.orederDetal .msgLis .specs{width:40%; color: #666;}
	.orederDetal .msgLis .num{width:40%; text-align: center;}
	.orederDetal .msgLis .mny{width: 60%; text-align: right;}
	
	/* 位置信息 */
	.GprsMsg .item .adrs{width: 90%;position: relative;box-sizing: border-box;padding-left: 15px;}
	.GprsMsg .item .dian{width: 6px; height: 6px;border-radius: 50%;position: absolute; top: 50%;left: 0;transform: translateY(-50%);}
	.GprsMsg .item .dian.red{background: #FF3B3B;}
	.GprsMsg .item .Ricon{width: 16px; height: 16px; display: block;}

</style>

