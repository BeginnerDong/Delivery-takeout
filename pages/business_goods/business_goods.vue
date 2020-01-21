<template>
	<div>
		
		<div class="pdlr4 mgt15">
			<div class="shopOrder">
				<div class="item radius5 boxShaow" v-for="(item,index) in mainData" :key="index">
					<div class="infor">
						<div class="ll" :data-id="item.id" >
							<img class="pic" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"/>
						</div>
						<div class="rr">
							<a class="titile fs13 avoidOverflow2" :data-id="item.id">
								{{item.title}}</a>
							<div class="Bmny">
								<div class="flex">
									<div class="price fs13">{{item.price}}</div>
									<div class="flex fs12 red mgl10 mgr10">
										<img class="zhe" src="../../static/images/home-icon4.png">{{item.discount}}折</div>
									<div class="yuanJia fs12">{{item.o_price}}</div>
								</div>
							</div>
						</div>
					</div>
					<div class="under_btn flexEnd pdt10 color6 fs12">
						<div class="btn flexEnd" :data-id="item.id"
						@click="Router.navigateTo({route:{path:'/pages/business_goods_edit/business_goods_edit?id='+$event.currentTarget.dataset.id}})"><img class="icon" src="../../static/images/management-icon.png">修改</div>
						<div class="btn flexEnd mgl40" 
						@click="deleteOne(index)"><img class="icon" src="../../static/images/management-icon.png" >删除</div>
						<div class="btn flexEnd mgl40" 
						@click="shelfOne(index)"><img class="icon" src="../../static/images/management-icon.png" >{{item.on_shelf==1?'下架':'上架'}}</div>
					</div>
				</div>
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
				mainData: [],
				proListDate:[
					{count:'1',addBtn:true,numBox:false},
					{count:'1',addBtn:false,numBox:true},
					{count:'1',addBtn:false,numBox:true}
				],
				is_Shelfshow:false
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
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
			deltAlert(){
				const self = this;
				self.is_show=!self.is_show;
			},
			
			deltShelf(){
				const self = this;
				self.is_Shelfshow=!self.is_Shelfshow;
			},
			
			deleteOne(index) {
				const self = this;
				uni.showModal({
				    title: '提示',
				    content: '确定删除这个商品吗',
				    success: function (res) {
				        if (res.confirm) {
				             const postData = {};
				            postData.searchItem = {
								thirdapp_id:3
							};
				            postData.searchItem.id = self.mainData[index].id;
				            postData.tokenFuncName = 'getStoreToken';
				            postData.data = {
				            	status:-1
				            };
				            const callback = (res) => {
				            	if (res.solely_code==100000) {
				            		self.$Utils.showToast('删除成功', 'none');
				            		setTimeout(function() {
				            			self.getMainData(true);
				            		}, 500);
				            		
				            	}else{
				            		self.$Utils.showToast(res.msg, 'none');
				            	}
				            };
				            self.$apis.productUpdate(postData, callback)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});		
			},
			
			shelfOne(index) {
				const self = this;
				uni.showModal({
				    title: '提示',
				    content: '确定下架（上架）这个商品吗',
				    success: function (res) {
				        if (res.confirm) {
				             const postData = {};
				            postData.searchItem = {
								thirdapp_id:3
							};
				            postData.searchItem.id = self.mainData[index].id;
				            postData.tokenFuncName = 'getStoreToken';
				            postData.data = {
				            	on_shelf:1
				            };
							if(self.mainData[index].on_shelf==1){
								postData.data.on_shelf = 0
							};
				            const callback = (res) => {
				            	if (res.solely_code==100000) {
				            		self.$Utils.showToast('操作成功', 'none');
				            		setTimeout(function() {
				            			self.getMainData(true);
				            		}, 500);
				            		
				            	}else{
				            		self.$Utils.showToast(res.msg, 'none');
				            	}
				            };
				            self.$apis.productUpdate(postData, callback)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});		
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getStoreToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 3,
				};		
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/agreeSel.css";
	
	
	.shopOrder{max-height: initial;  overflow-y: initial;}
	.shopOrder .item{padding: 15px 4%;}
	.shopOrder .item .infor{padding-left: 90px;}
	.shopOrder .item .ll{width: 80px; height: 80px;}
	.shopOrder .item .rr{height: 80px;}
	.under_btn .icon{width: 14px; height: 14px; display: block;margin-right: 5px;}
</style>

