<template>
	<div>
		<div class="orderNav" style="top:0">
			<div class="tt" :class="current==1?'on':''" @click="change('1')">提现审核</div>
			<div class="tt" :class="current==2?'on':''" @click="change('2')">提现流水</div>
		</div>
		
		<div class="pdlr4 bs-CashLis" style="margin-top: 55px;" v-show="current==1">
			<ul>
				<li class="radius5 boxShaow flexRowBetween" v-for="(item,index) in mainData" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/business_cashOutDetail/business_cashOutDetail?id='+$event.currentTarget.dataset.id}})">
					<div class="ll">
						<p class="flex pdb10">
							<span class="tt color6">提现金额</span>
							<span class="red">{{item.count}}</span>
						</p>
						<p class="flex">
							<span class="tt color6">姓名</span>
							<span class="color2">{{item.card&&item.card[0]?item.card[0].name:''}}</span>
						</p>
					</div>
					<div class="rr">
						<a class="flexEnd pdtb20" >
							<img class="arrowR" src="../../static/images/icon.png" >
						</a>
					</div>
				</li>
			</ul>
		</div>
		
		<div class="pdlr4 bs-CashLis" style="margin-top: 55px;" v-show="current==2">
			<ul>
				<li class="radius5 boxShaow" v-for="(item,index) in mainData">
					<p class="flex pdb10">
						<span class="tt color6">提现金额</span>
						<span class="red">{{item.card}}</span>
					</p>
					<p class="flex pdb10">
						<span class="tt color6">姓名</span>
						<span class="color2">{{item.card&&item.card[0]?item.card[0].name:''}}</span>
					</p>
					<p class="flex">
						<span class="tt color6">时间</span>
						<span class="color2">{{item.create_time}}</span>
					</p>
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
				CashLisData:[{},{},{}],
				searchItem:{
					type:2,
					user_type:0,
					withdraw_status:0,
					withdraw:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self)
		
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
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						self.searchItem.withdraw_status = 0
					}else if(self.current==2){
						self.searchItem.withdraw_status = 1
					}
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
				postData.tokenFuncName = 'getStoreToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					card:{
						tableName:'UserCard',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/order.css";
	@import "../../assets/style/agreeSel.css";
	
	page{background: #F5F5F5;padding-bottom: 30px}
	.orderNav{ position: fixed;top: 50px;left: 0; width: 100%;box-sizing: border-box;background: #fff; z-index: 5;border-bottom: 1px solid #eee;padding: 0 4%;}
	.orderNav .tt{width: 50%;}
	.orderNav .on::after{width: 50px; }
	.bs-CashLis li{padding: 15px; background: #fff; margin-bottom: 15px;}
	.bs-CashLis li .tt{width: 80px;}
	.bs-CashLis li .rr{width: 15%; display: block;}
</style>

