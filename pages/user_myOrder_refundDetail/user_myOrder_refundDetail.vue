<template>
	<div>
		
		<div class="orderList f5bj" style="padding: 15px 4%;">
			<ul>
				<li style="margin-top: 0;">
					<div class="infor">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：{{mainData.create_time}}</div>
							<div class="state" v-if="mainData.transport_status==0&&mainData.order_step==0">待接单</div>
							<div class="state" v-if="mainData.transport_status==1&&mainData.order_step==0">商家已接单</div>
							<div class="state" v-if="mainData.transport_status==2&&mainData.order_step==0">配送中</div>
							<div class="state" v-if="mainData.transport_status==3&&mainData.order_step==0">已完成</div>
							<div class="state" v-if="mainData.order_step==1">退款中</div>
							<div class="state" v-if="mainData.order_step==2">退款完成</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12" v-if="mainData.type==5">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">{{mainData.title}}</span>
								<span class="num">×{{mainData.count}}</span>
								<span class="mny">￥{{mainData.price}}</span>
							</p>
						</div>
						<div class="msg mglr10 pr radius5 fs12" v-if="mainData.type==6">
							<p class="child  flexRowBetween" v-for="c_item in mainData.child[0]">
								<span class="tit avoidOverflow">{{c_item.title}}</span>
								<span class="num">×{{c_item.count}}</span>
								<span class="mny">￥{{c_item.unit_price}}</span>
							</p>
						</div>
					</div>
				</li>
			</ul>
		</div>
		
		<div class="pdlr4">
			<div class="line40">退款原因</div>
			<div>
				<textarea disabled="disabled" class="w radius5 fs12 color6" style=" height: 100px;box-sizing: border-box;border: none; background: #f8f8f8;padding: 15px; display: block;" rows="" cols="" 
				v-model="submitData.reason" />
			</div>
			<div class="line40 mgt15" v-if="submitData.reply!=''">商家回复</div>
			<div class="pd10 f5bj radius5" v-if="submitData.reply!=''">
				<textarea  disabled="disabled" auto-height class="w fs12 color6" style="line-height: 24px;border: none;display: block;" 
				 v-model="submitData.reply" />
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
				submitData:{
					reason:'',
					reply:''
				}
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
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.reason = self.mainData.reason;
						self.submitData.reply = self.mainData.reply;
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
	@import "../../assets/style/order.css";
	
</style>

