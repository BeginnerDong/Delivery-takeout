<template>
	<div>
		<div class="pdlr4">
			<!-- banner -->
			<div class="ind_banner oh pdtb15 radius10">
				<img :src="userInfoData.mainImg&&userInfoData.mainImg[0]?userInfoData.mainImg[0].url:''" alt="">
			</div>
			<div class="inde_msgBox">
				<ul>
					<li class="flex">
						<img class="Licon" src="../../static/images/home-icon2.png">
						<span class="fs14">{{userInfoData.name}}</span>
						<span>{{userInfoData.passage1}}</span>
					</li>
					<li class="flexRowBetween">
						<div class="flex">
							<img class="Licon" src="../../static/images/home-icon1.png">
							<span class="color6 fs12">{{userInfoData.address}}</span>
						</div>
						<img class="RadrsIcon" src="../../static/images/home-icon3.png" >
					</li>
					<li class="flex">
						<img class="Licon" src="../../static/images/home-icon.png">
						<span class="color6 fs12">{{userInfoData.phone}}</span>
					</li>
				</ul>
			</div>
		</div>
		<div class="f5H5"></div>
		
		<div class="inde_prolis pr" style="min-height: 260px;">
			<scroll-view class="store_LeftNav center"  scroll-y="true" :style="'height:'+(windowHeight*2 - 380)+'rpx'">
				<div class="item" v-for="(item,index) in labelData" 
				:class="curr==index?'on':''"  :key="index" @click="changeNav(index)">
				{{item.title}}
				</div>
			</scroll-view>
			<scroll-view scroll-y="true"  :scroll-into-view="currId" @scroll="mainScroll"  scroll-with-animation="true" class="store_rightCont"  
			:style="'height:'+(windowHeight*2 - 380)+'rpx'">
				<div class="shopOrder" v-for="(item,index) in mainData" :id="item.id">
					<div style="width: 100%;background: #2FA0ED;color: #fff;text-align: center;border-radius: 10rpx;">{{item.menu}}</div>
					<div class="item">
						<div class="infor" style="margin: 20rpx 0;"  v-for="(c_item,c_index) in item.data" :key="c_index">
							<div class="ll" :data-id="c_item.id">
								<img class="pic" :src="c_item.mainImg&&c_item.mainImg[0]?c_item.mainImg[0].url:''"/>
							</div>
							<div class="rr">
								<div class="titile fs13 avoidOverflow2" :data-id="c_item.id"
								@click="Router.navigateTo({route:{path:'/pages/goodsDetail/goodsDetail?id='+$event.currentTarget.dataset.id}})">
									{{c_item.title}}
								</div>
								<div class="flexRowBetween Bmny">
									<div class="flex">
										<div class="price fs13">{{c_item.price}}</div>
										<!-- <div class="flex fs12 red mgl10 mgr10"><img class="zhe" src="../../static/images/home-icon4.png">{{c_item.discount}}折</div> -->
										<div class="yuanJia fs12" v-if="c_item.o_price!=''">{{c_item.o_price}}</div>
									</div>
									
									<div class="flexEnd">
										<div class="addBtn font14" v-if="c_item.count==0"  @click="counter(index,c_index,'+')">+</div>
										<div class="numBox flexEnd"  v-if="c_item.count>0">
											<div class="" @click="counter(index,c_index,'-')">-</div>
											<div class="num">{{c_item.count}}</div>
											<div class="on" @click="counter(index,c_index,'+')">+</div>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</scroll-view>
		</div>
		
		<div class="xqbotomBar" >
			<div class="threeNav center w flex">
				<a class="item L flexCenter fs12" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<div class="flexColumn">
						<div class="mgt5"><img class="L-icon" src="../../static/images/home-icon7.png" ></div>
						<div>我的</div>
					</div>
				</a>
				<div class="item M flexCenter">
					<div class="mgr10">
						<img class="CarIcon" src="../../static/images/home-icon8.png" mode=""/>
					</div>
					<div class="fs12 flexColumn">
						<div class="pr price" v-if="cartData.length>0" @click="carMxShow">
						<span class="pr">{{totalPrice}}<em class="car-num">{{cartData.length}}</em></span></div>
					<!-- <p>配送费￥10</p>	 -->
					<p v-if="cartData.length==0">购物车是空的~</p>
					</div>
				</div>
				<a class="item R flexCenter white" @click="goBuy()">去结算</a>
			</div>
		</div>
		
		<!-- 弹框背景 -->
		<div class="black-bj" v-show="is_show" @click="carMxShow"></div>
		
		<!-- 费用明细 -->
		<div class="qjAlertCont carMxShow" style="padding-bottom:60px;" v-show="is_priceShow">
			<div class="flexRowBetween pdlr4 line40 bordB1 fs12 color6 f5bj">
				<!-- <span>配送费10元</span> -->
				<span class="flex" @click="deleteAll()">
					<img class="arrowR mgr5" src="../../static/images/management-icon1.png" style="width: 14px; height: 14px; display: block;" >清空购物车
				</span>
			</div>
			<div class="pdlr4 list pdt10">
				<ul>
					<li class="flexRowBetween pdb15 fs13" v-for="(item,index) in cartData" :key='index'>
						<span class="L avoidOverflow">{{item.title}}</span>
						<span class="M">￥{{item.price}}</span>
						<span class="R numBox flexEnd">
							<div class="" @click="counterTwo(index,'-')">-</div>
							<div class="num">{{item.count}}</div>
							<div class="on" @click="counterTwo(index,'+')">+</div>
						</span>
					</li>
				</ul>
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
				curr:0,
				leftNavDate:['折扣','热销','套餐','饮品'],
				proListDate:[
					{count:'1',addBtn:true,numBox:false},
					{count:'1',addBtn:false,numBox:true},
					{count:'1',addBtn:false,numBox:true}
				],
				priceShow:false,
				moneyMxDate:[{},{},{},{},{},{},{},{},{}],
				is_priceShow:false,
				userInfoData:{},
				labelData:[],
				idArray:[],
				mainData:[],
				cartData:[],
				hasOne:false,
				totalPrice:0,
				originData:[],
				windowHeight:'',
				currId:''
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.removeStorageSync('cartDataTwo');
			const res = uni.getSystemInfoSync();
			self.windowHeight = res.windowHeight;
			console.log(res.windowHeight);
/* 			if(options.scene){
				var scene = decodeURIComponent(options.scene);
				self.table_no = scene;
			}; */
			if(options.user_no){
				const callback = (res) => {
					self.$Utils.loadAll(['getLabelData','getUserInfoData','thirdAppGet'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:options.user_no
				})
			}else{
				const callback = (res) => {
					self.$Utils.loadAll(['getLabelData','getUserInfoData','thirdAppGet'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true
				})
			}
			
		},
		
		onShow() {
			const self = this;
			self.cartData = self.$Utils.getStorageArray('cartDataTwo');
			console.log('self.cartData',self.cartData)
			for (var i = 0; i < self.cartData.length; i++) {
				for (var j = 0; j < self.originData.length; j++) {
					if(self.cartData[i].id==self.originData[j].id){
						self.originData[j].count = self.cartData[i].count
					}
				}
			};
			console.log('self.originData',self.originData)
			self.mainData = [];
			console.log('aaaaaa?')
			for (var i = 0; i < self.originData.length; i++) {
				if(self.mainData.length>0){
					var hasone = false;
					for(var j =0;j<self.mainData.length;j++){
						if(self.originData[i].label[self.originData[i].category_id].title==self.mainData[j].menu){
							self.mainData[j].data.push(self.originData[i]);
							hasone = true;
						};
					};
					if(!hasone){
						self.mainData.push({
							menu: self.originData[i].label[self.originData[i].category_id].title,
							id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
							data:[self.originData[i]]
						});
					};
				}else{
					self.mainData.push({
						menu: self.originData[i].label[self.originData[i].category_id].title,
						id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
						data:[self.originData[i]]
					})
				};
			};
			for (var i = 0; i < self.mainData.length; i++) {
				if(i>0){
					self.mainData[i].height = self.mainData[i].data.length*70+50+self.mainData[i-1].height
				}else{
					self.mainData[i].height = self.mainData[i].data.length*70+50
				}
				
			}
			self.countPrice();
			console.log('self.cartData',self.cartData)
		},
		

		
		methods: {
			
			
			
			onShareAppMessage: function(ops) {
				console.log(ops)
				const self = this;
				if (ops.from === 'button') {
					
				}else{
					return {
						title: '好鲜生配送到家',
						path: '/pages/index/index?user_no='+uni.getStorageSync('user_info').user_no, //点击分享的图片进到哪一个页面
						success: function(res) {
							// 转发成功
							console.log("转发成功:" + JSON.stringify(res));
						},
						fail: function(res) {
							// 转发失败
							console.log("转发失败:" + JSON.stringify(res));
						}
					}
					console.log(ops.target)
				}
				
			},
			
			goBuy(){
				const self = this;
				if(self.cartData.length==0){
					self.$Utils.showToast('购物车是空的啊', 'none')
					return
				};
				if(parseFloat(self.totalPrice)<parseFloat(self.thirdAppData.custom_rule.limit_price)){
					uni.showModal({
					    title: '提示',
					    content: '起送价'+parseFloat(self.thirdAppData.custom_rule.limit_price),
						confirmColor:'#ca1c1d',
					    success: function (res) {
					        if (res.confirm) {
					           
					        } else if (res.cancel) {
					           
					        }
					    }
					});	
				}else{
					self.Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})
				}
			},
			
			thirdAppGet() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.thirdAppData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('thirdAppGet');
				};
				self.$apis.thirdAppGet(postData, callback);
			},
			
			deleteAll() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认要清空购物车吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							for (var i = 0; i < self.cartData.length; i++) {
								self.$Utils.delStorageArray('cartDataTwo', self.cartData[i], 'id');
							};
							self.cartData = self.$Utils.getStorageArray('cartDataTwo');
							self.carMxShow()
							console.log('self.cartData',self.cartData)
							for (var i = 0; i < self.cartData.length; i++) {
								for (var j = 0; j < self.originData.length; j++) {
									if(self.cartData[i].id==self.originData[j].id){
										self.originData[j].count = self.cartData[i].count
									}
								}
							};
							console.log('self.originData',self.originData)
							self.mainData = [];
							console.log('aaaaaa?')
							for (var i = 0; i < self.originData.length; i++) {
								if(self.mainData.length>0){
									var hasone = false;
									for(var j =0;j<self.mainData.length;j++){
										if(self.originData[i].label[self.originData[i].category_id].title==self.mainData[j].menu){
											self.mainData[j].data.push(self.originData[i]);
											hasone = true;
										};
									};
									if(!hasone){
										self.mainData.push({
											menu: self.originData[i].label[self.originData[i].category_id].title,
											id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
											data:[self.originData[i]]
										});
									};
								}else{
									self.mainData.push({
										menu: self.originData[i].label[self.originData[i].category_id].title,
										id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
										data:[self.originData[i]]
									})
								};
							};
							for (var i = 0; i < self.mainData.length; i++) {
								if(i>0){
									self.mainData[i].height = self.mainData[i].data.length*70+50+self.mainData[i-1].height
								}else{
									self.mainData[i].height = self.mainData[i].data.length*70+50
								}
								
							}
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			},
			
			addBtnShow(){
				const self = this;
				self.priceShow = true
			},
			
			changeNav(index){
				const self = this;
				self.curr = index;
				self.currId = self.labelData[self.curr].id;
				console.log(self.currId)
			},
			
			mainScroll(e){
				console.log(e.detail.scrollTop);
				const self = this;
				for (var i = 0; i < self.mainData.length; i++) {
					if(e.detail.scrollTop>self.mainData[i].height&&e.detail.scrollTop<self.mainData[i+1].height){
						self.curr = i+1
						console.log('curr',self.curr)
					}
					if(e.detail.scrollTop<self.mainData[0].height){
						self.curr = 0
					}
				}
			},
			
			carMxShow(){
				const self = this;
				self.is_show = !self.is_priceShow;
				self.is_priceShow = !self.is_priceShow
			},
			
			counter(index,c_index,type) {
				const self = this;
				self.mainData = self.$Utils.cloneForm(self.mainData)
				if (type == '+') {
					
					var findItem = self.$Utils.findItemInArray(self.cartData, 'id', self.mainData[index].data[c_index].id);
					if(self.mainData[index].data[c_index].count==0){
						self.mainData[index].data[c_index].count++;
						self.cartData.push(self.mainData[index].data[c_index])
					}else{
						
						self.cartData[findItem[0]].count++
						console.log('32423',self.cartData[findItem[0]].count)
						self.mainData[index].data[c_index].count++;
					};
					
					console.log('self.cartData',self.cartData)
					console.log('self.mainData',self.mainData)
				}else{
					if(self.mainData[index].data[c_index].count >= 1) {
						var findItem = self.$Utils.findItemInArray(self.cartData, 'id', self.mainData[index].data[c_index].id);
						if(self.mainData[index].data[c_index].count==1){
							self.cartData.splice(findItem[0], 1);
						}else{
					
							self.cartData[findItem[0]].count--
						}
						self.mainData[index].data[c_index].count--;
					}
				};
				self.countPrice()
			},
			
			counterTwo(index,type) {
				const self = this;
				var findItem = self.$Utils.findItemInArray(self.originData, 'id', self.cartData[index].id);
				console.log('findItem',findItem)
				//return
				if (type == '+') {
					
					
					var childItem = self.$Utils.cloneForm(self.cartData);
					
					childItem[index].count++;
					self.cartData = childItem
					
					self.originData[findItem[0]].count++
					console.log('self.cartData',self.cartData)
					//return
				}else{
					if(self.cartData[index].count >= 1) {
						var childItem = self.$Utils.cloneForm(self.cartData);
						childItem[index].count--;
						self.originData[findItem[0]].count--
						if(childItem[index].count==0){
							childItem.splice(index,1)
							self.originData[findItem[0]].count = 0;
						};
						self.$Utils.delStorageArray('cartDataTwo', self.cartData[index], 'id')
						self.cartData = childItem
					}
				};
				self.mainData = [];
				console.log('aaaaaa?')
				for (var i = 0; i < self.originData.length; i++) {
					if(self.mainData.length>0){
						var hasone = false;
						for(var j =0;j<self.mainData.length;j++){
							if(self.originData[i].label[self.originData[i].category_id].title==self.mainData[j].menu){
								self.mainData[j].data.push(self.originData[i]);
								hasone = true;
							};
						};
						if(!hasone){
							self.mainData.push({
								menu: self.originData[i].label[self.originData[i].category_id].title,
								id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
								data:[self.originData[i]]
							});
						};
					}else{
						self.mainData.push({
							menu: self.originData[i].label[self.originData[i].category_id].title,
							id:'a'+ self.originData[i].label[self.originData[i].category_id].id,
							data:[self.originData[i]]
						})
					};
				};
				for (var i = 0; i < self.mainData.length; i++) {
					if(i>0){
						self.mainData[i].height = self.mainData[i].data.length*70+50+self.mainData[i-1].height
					}else{
						self.mainData[i].height = self.mainData[i].data.length*70+50
					}
					
				}
				console.log('23243',self.$Utils.getStorageArray('cartDataTwo'))
				self.countPrice()
			},
			
			countPrice(){
				const self = this;
				self.totalPrice = 0;
				uni.removeStorageSync('cartDataTwo');
				for (var i = 0; i < self.cartData.length; i++) {
					self.totalPrice += parseInt(self.cartData[i].count)*parseFloat(self.cartData[i].price);
					self.$Utils.setStorageArray('cartDataTwo',self.cartData[i], 'id', 999);
				}
				
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
					user_type:2
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						 self.userInfoData = res.info.data[0];
						 uni.setStorageSync('storeAddress',self.userInfoData)
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
					type:3
				};
				postData.order = {
					create_time:'asc'
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						};
						self.getMainData()
					} 
					console.log('self.labelData',self.labelData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
					category_id:['in',self.idArray],
					on_shelf:1
				};
				postData.order = {
					category_id:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.originData = res.info.data;
						for (var i = 0; i < self.labelData.length; i++) {
							self.labelData[i].id = 'a'+self.labelData[i].id
						};
						self.currId = self.labelData[0].id;
						
						console.log('self.curr',self.curr)
						for (var i = 0; i < res.info.data.length; i++) {
							res.info.data[i].count=0;
							
							if(self.mainData.length>0){
								var hasone = false;
								for(var j =0;j<self.mainData.length;j++){
									if(res.info.data[i].label[res.info.data[i].category_id].title==self.mainData[j].menu){
										self.mainData[j].data.push(res.info.data[i]);
										hasone = true;
									};
								};
								if(!hasone){
									self.mainData.push({
										menu: res.info.data[i].label[res.info.data[i].category_id].title,
										id:'a'+ res.info.data[i].label[res.info.data[i].category_id].id,
										data:[res.info.data[i]],
										
									});
								};
							}else{
								self.mainData.push({
									menu: res.info.data[i].label[res.info.data[i].category_id].title,
									id:'a'+ res.info.data[i].label[res.info.data[i].category_id].id,
									data:[res.info.data[i]],
									
								})
							};
						};
						
						for (var i = 0; i < self.mainData.length; i++) {
							if(i>0){
								self.mainData[i].height = self.mainData[i].data.length*70+50+self.mainData[i-1].height
							}else{
								self.mainData[i].height = self.mainData[i].data.length*70+50
							}
							
						}
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
	@import "../../assets/style/numCar.css";
	@import "../../assets/style/prodctDetail.css";

</style>

