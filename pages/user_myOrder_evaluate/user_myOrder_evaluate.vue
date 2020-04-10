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
							<p class="child  flexRowBetween" v-for="item in mainData.child[0]">
								<span class="tit avoidOverflow">{{item.title}}</span>
								<span class="num">×{{item.count}}</span>
								<span class="mny">￥{{item.unit_price}}</span>
							</p>
						</div>
					</div>
				</li>
				
			</ul>
		</div>
		
		<div class="pdlr4">
			<div class="line40">填写评价</div>
			<div>
				<textarea class="w radius5 fs12" v-model="submitData.content" style="height: 160px;box-sizing: border-box;border: none; background: #f8f8f8;padding: 15px; display: block;" rows="" cols="" placeholder="请写下您对这次交易的看法" value=""/>
			</div>
		</div>
		<div class="submitbtn" style="margin-top: 60px;">
			<button class="btn" type="submit" @click="Utils.stopMultiClick(submit)">提交</button>
		</div>
		
	</div>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				is_show:false,
				mainData: {},
				submitData:{
					mainImg:[],
					
					order_no:'',
					relation_id:'',
					content:'',
					type:2,
					title:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
			self.submitData.mainImg = [{type:'image',url:uni.getStorageSync('user_info').headImgUrl}];
			self.submitData.title = uni.getStorageSync('user_info').nickname
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
						if(self.mainData.type==5){
							self.submitData.relation_id = self.mainData.product_id
						}else{
							self.submitData.relation_id = self.mainData.child[0][0].id
						}
						self.submitData.order_no = self.mainData.order_no
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [];
				if(self.mainData.type==5){
					postData.saveAfter.push(
						{
						  tableName:'Order',
						  FuncName:'update',
						  searchItem:{
							order_no:self.mainData.order_no
						  },
						  data:{
							isremark:1,
							
						  }
						}
					)
				}else if(self.mainData.type==6){
					for (var i = 0; i < self.mainData.child[0].length; i++) {
						postData.saveAfter.push(
							{
							  tableName:'Order',
							  FuncName:'update',
							  searchItem:{
								order_no:self.mainData.child[0][i].order_no
							  },
							  data:{
								isremark:1,
								
							  }
							}
						)
					}
				}
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
			
		},
	}
</script>

<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/order.css";
	
	

</style>

