<template>
	<div>
		
		<div class="myExtendTop white">
			<div class="bigNum">{{userInfoData.balance}}</div>
			<div class="yuan">收益(元)</div>
			<a class="txBtn"  @click="Router.navigateTo({route:{path:'/pages/user_myCashOut/user_myCashOut'}})">提现</a>
		</div>
		
		<div class="myRowBetween" v-if="mainData.length>0">
			<div class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<div class="left">
					<div class="avoidOverflow">{{item.trade_info}}
					<span style="font-size:12px;color: red;" v-if="item.status==0">(审核中)</span></div>
					<div class="time">{{item.create_time}}</div>
				</div>
				<div class="right" :class="item.count>0?'red':'pucolor'">{{item.count}}</div>
			</div>
		</div>
	</div>
</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				rewardData:[
					{},{},{}
				],
				searchItem:{
					type:2,
					status:['in',[0,1]]
				},
				userInfoData:{},
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self)
		
		},
			
		onShow() {
			const self =  this;
			self.getUserInfoData();
			self.getMainData(true);
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
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no		
				
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getProjectToken'
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log(self.mainData)
				};
				self.$apis.flowLogGet(postData, callback);
			},

		},
	};
</script>


<style>
	@import "../../assets/style/public.css";
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; padding: 0 4%; line-height:44px;}
	.myExtendTop .yuan{ font-size:14px; padding:5px 0 15px 0;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 10px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .time{color: #999; font-size: 12px;}
	.myRowBetween .item .left .time{margin-top: 5px;}
	.myRowBetween .item .right{width: 50%;text-align: right;font-size: 15px; display: flex; justify-content: flex-end;align-items: center;}

</style>

