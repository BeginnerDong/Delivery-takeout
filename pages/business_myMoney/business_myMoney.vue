<template>
	<div>
		
		<div class="myExtendTop white center">
			<div class="bigNum mgb5">{{userInfoData.balance}}</div>
			<div class="yuan">总收益</div>
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
		
		
		<div class="myRowBetween" >
			<div class="item flexRowBetween fs13 color6" v-for="item in mainData">
				<div class="left ">{{item.create_time}}</div>
				<div class="right flexEnd">￥{{item.count}}</div>
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
					status:['in',[1]],
					
				},
				userInfoData:{},
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				},
				end:'',
				start:''
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
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getStoreToken'
				postData.searchItem.user_no = uni.getStorageSync('storeInfo').user_no		
				
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
				postData.tokenFuncName = 'getStoreToken'
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
	@import "../../assets/style/user.css";
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4% 60px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; line-height:40px;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 15px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .right{width: 50%;}
</style>

