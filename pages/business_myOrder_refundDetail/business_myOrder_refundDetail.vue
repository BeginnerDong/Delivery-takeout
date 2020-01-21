<template>
	<div>
		
		<div class="orderList f5bj" style="padding: 15px 4%;">
			<ul>
				<li style="margin-top: 0;">
					<div class="infor">
						<div class="datt flexRowBetween bordB1 mgb10">
							<div class="left color9">交易时间：{{mainData.create_time}}</div>
							<div class="state" v-if="mainData.transport_status==0&&mainData.order_step==0">待接单</div>
							<div class="state" v-if="mainData.transport_status==1&&mainData.order_step==0">已接单</div>
							<div class="state" v-if="mainData.transport_status==2&&mainData.order_step==0">已配送</div>
							<div class="state" v-if="mainData.transport_status==3&&mainData.order_step==0">已完成</div>
							<div class="state" v-if="mainData.order_step==1">待退款</div>
							<div class="state" v-if="mainData.order_step==2">已退款</div>
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
				<textarea  disabled="disabled" class="w radius5 fs12 color6" 
				style=" height: 100px;box-sizing: border-box;border: none; background: #f8f8f8;padding: 10px; display: block;color: #333;" 
				rows="" cols="" v-model="reason" />
			</div>
		</div>
		<div class="pdlr4 mgt15">
			<div class="line40">商家回复</div>
			<div>
				<textarea   class="w radius5 fs12 color6" 
				style=" height: 100px;box-sizing: border-box;border: none; background: #f8f8f8;padding:15px; display: block;" rows="" cols="" placeholder="请输入您的退款结果" v-model="submitData.reply"/>
			</div>
		</div>
		
		<div class="submitbtn" style="margin-top: 60px;">
			<button class="btn" type="submit" @click="submit('1')">同意</button>
			<button class="btn" type="submit" @click="submit('2')">拒绝</button>
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
					reply:'',
				},
				reason:''
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
						self.mainData = res.info.data[0],
						self.reason = self.mainData.reason
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			orderUpdate(type) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.searchItem = {
					id:self.id
				};
				if(type=='1'){
					postData.data.order_step=2
				}else{
					postData.data.order_step=3
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('申请成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
					
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
			
			submit(type) {
				const self = this;
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					self.orderUpdate(type)
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入回复信息','success');
				};
			},
			
		}
		
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/order.css";
	
</style>

