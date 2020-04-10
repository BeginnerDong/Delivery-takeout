<template>
	<div>
		
		<div class="orderNav" style="top:0">
			<div class="tt" :class="current==1?'on':''" @click="change('1')">全部订单</div>
			<div class="tt" :class="current==2?'on':''" @click="change('2')">待配送</div>
			<div class="tt" :class="current==3?'on':''" @click="change('3')">配送中</div>
			<div class="tt" :class="current==4?'on':''" @click="change('4')">已完成</div>
			<div class="tt" :class="current==5?'on':''" @click="change('5')">退款</div>
		</div>
		
		<div class="orderList pdlr4" style="margin-top: 60px;">
			<ul>
				<li v-for="(item,index) in mainData">
					<div class="infor" :data-id="item.id"
					 @click="Router.navigateTo({route:{path:'/pages/user_myOrder_Detail/user_myOrder_Detail?id='+$event.currentTarget.dataset.id}})">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：{{item.create_time}}</div>
							<div class="state" v-if="item.transport_status==0&&item.order_step==0">待接单</div>
							<div class="state" v-if="item.transport_status==1&&item.order_step==0">商家已接单</div>
							<div class="state" v-if="item.transport_status==2&&item.order_step==0">配送中</div>
							<div class="state" v-if="item.transport_status==3&&item.order_step==0">已完成</div>
							<div class="state" v-if="item.order_step==1">退款中</div>
							<div class="state" v-if="item.order_step==2">退款完成</div>
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
					<div class="underBtn pdlr10 mgt15">
						<span class="Bbtn" v-if="(item.transport_status==0||item.transport_status==1)&&item.order_step==0" :data-id="item.id"
						 @click="Router.navigateTo({route:{path:'/pages/user_myOrder_refund/user_myOrder_refund?id='+$event.currentTarget.dataset.id}})">
							退款
						</span>
						<span class="Bbtn" v-if="item.order_step!=0" :data-id="item.id"
						  @click="Router.navigateTo({route:{path:'/pages/user_myOrder_refundDetail/user_myOrder_refundDetail?id='+$event.currentTarget.dataset.id}})">
							查看详情
						</span>     
						<span class="Bbtn" v-if="item.transport_status==3&&item.isremark==0" :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/user_myOrder_evaluate/user_myOrder_evaluate?id='+$event.currentTarget.dataset.id}})">
						去评价
						</span>
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
				current:1,
				searchItem:{
					thirdapp_id: 3,
					level:0,
					pay_status:1
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
			if(options.num){
				self.num = options.num
			}
		},
		
		onShow() {
			const self = this;
			self.mainData = [];
			if(self.num){
				self.change(self.num)
			}else{
				self.$Utils.loadAll(['getMainData'], self);
			}
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
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
					if(self.current==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.order_step
					}else if(self.current==2){
						self.searchItem.transport_status = ['in',[0,1]]
						self.searchItem.order_step = 0
					}else if(self.current==3){
						self.searchItem.transport_status = 2
						self.searchItem.order_step = 0
					}else if(self.current==4){
						self.searchItem.transport_status = 3
						self.searchItem.order_step = 0
					}else if(self.current==5){
						self.searchItem.order_step = ['not in',[0]]
					};
					self.getMainData(true)
				}
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.orderNav{ position: fixed;top: 50px;left: 0; width: 100%;box-sizing: border-box;background: #fff; z-index: 5;border-bottom: 1px solid #eee;}

</style>

