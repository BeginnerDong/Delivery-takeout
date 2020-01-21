<template>
	<div>
		<div class="myRowBetween" >
			<ul>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">提现金额</p>
					<p class="right flexEnd red fs13">{{mainData.count}}</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">姓名</p>
					<p class="right flexEnd">{{mainData.card&&mainData.card[0]?mainData.card[0].name:''}}</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">手机号</p>
					<p class="right fs13 flexEnd">{{mainData.card&&mainData.card[0]?mainData.card[0].phone:''}}</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">开户行</p>
					<p class="right fs13 flexEnd">{{mainData.bank?mainData.bank.title:''}}</p>
				</li>
				<li class="item flexRowBetween" >
					<p class="left fs14 color6">银行卡号</p>
					<p class="right fs13 flexEnd">{{mainData.card&&mainData.card[0]?mainData.card[0].number:''}}</p>
				</li>
			</ul>
		</div>
		
		<div class="pdlr4 submitbtn flexRowBetween" style="margin-top: 100px;">
			<button class="btn" style="width: 43%;margin: 0 10px;background: #addaf9;" type="button" @click="flowLogUpdate('-1')">取消</button>
			<button class="btn" style="width: 43%;margin: 0 10px;" type="button" @click="flowLogUpdate('1')">确定</button>
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
				mainData: {}
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
				postData.tokenFuncName = 'getStoreToken';
				postData.searchItem = 
				postData.getAfter = {
					card:{
						tableName:'Card',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					bank:{
						tableName:'Label',
						middleKey:['card',0,'bank'],
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			flowLogUpdate(type) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getStoreToken';
				postData.data = {
					withdraw_status:type
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				if(type=='-1'){
					postData.data.status = -1
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
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
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	
</style>

