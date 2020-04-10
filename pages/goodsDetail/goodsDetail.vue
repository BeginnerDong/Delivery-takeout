<template>
	<div>
		
		<!-- banner -->
		<div class="detailxqBan oh">
			<swiper class="swiper-box"  autoplay="true" interval="5000" duration="1000" style="width: 100%;height: 400rpx;">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" style="width: 100%;height: 400rpx;"/>
					</swiper-item>
				</block>
			</swiper>
			
		</div>
		
		<div class="pdlr4 pdt10 pdb10">	
			<h1 class="ftn">{{mainData.title}}</h1>
			<p class="pdt5 flexRowBetween">
				<span class="price">{{mainData.price}}</span>
				<!-- <span class="addCar fs12 flexEnd color6" @click="collect()">
					<img style="width: 15px; height: 15px; display: block;margin-right: 5px;" 
					src="../../static/images/goods-icon.png" alt=""/>加入购物车
				</span> -->
			</p>
		</div>
		
		<div class="f5H10"></div>
		<div class="xqInfor">
			<div class="cont">
				<h2 class="ftn title pdb5">商品详情</h2>
				<p>{{mainData.description}}</p>
			</div>
		</div>
		<div class="f5H10"></div>
		
		<div class="pdlr4 pdt15">
			<h2 class="ftn title">商品评论</h2>
			<div class="detail_pj">
				<ul v-if="messageData.length>0">
					<li class="bordB1 pdtb10" v-for="(item,index) in messageData" :key="index">
						<div class="flexRowBetween fs12">
							<div class="flex name color6 pdb5">
								<image class="photo" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" />
								<span class="flexRowBetween">{{item.title}}</span>
							</div>
							<div class="time color9 flexEnd">{{item.create_time}}</div>
						</div>
						<h2 class="ftn text fs13">{{item.content}}</h2>
					</li>
					
				</ul>
				<ul v-else style="width: 100%;text-align: center;margin-top: 50rpx;">
					<div>暂无评论~</div>
				</ul>
			</div>
			
		</div>
		
		<!-- <div class="xqbotomBar" >
			<div class="threeNav center w flex">
				<div class="item L flexCenter fs12"  @click="Router.navigateTo({route:{path:'/pages/user/user'}})">
					<div class="flexColumn">
						<div class="mgt5"><image class="L-icon" src="../../static/images/home-icon7.png" /></div>
						<div>我的</div>
					</div>
				</div>
				<div class="item M flexCenter">
					<div class="mgr10">
						<image class="CarIcon" src="../../static/images/home-icon8.png" mode=""/>
					</div>
					<div class="fs12 flexColumn">
						<div class="pr price" v-show="!priceShow" @click="carMxShow"><span class="pr">59<em class="car-num">1</em></span></div>
						<p>配送费￥10</p>
					</div>
				</div>
				<a class="item R flexCenter white" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">去结算</a>
			</div>
		</div> -->
		
		<!-- 弹框背景 -->
		<!-- <div class="black-bj" v-show="is_show"></div> -->
		
		<!-- 费用明细 -->
		<!-- <div class="qjAlertCont carMxShow" style="padding-bottom:60px;" v-show="is_priceShow">
			<div class="flexRowBetween pdlr4 line40 bordB1 fs12 color6 f5bj">
				<span>配送费10元</span>
				<span class="flex">
					<image class="arrowR mgr5" src="../../static/images/management-icon1.png" style="width: 14px; height: 14px; display: block;" />清空购物车
				</span>
			</div>
			<div class="pdlr4 list pdt10">
				<ul>
					<li class="flexRowBetween pdb15 fs13" v-for="(item,index) in moneyMxDate" :key='index'>
						<span class="L avoidOverflow">外婆资质萝卜干</span>
						<span class="M">￥56.22</span>
						<span class="R numBox flexEnd">
							<div class="">-</div>
							<div class="num">1</div>
							<div class="on">+</div>
						</span>
					</li>
				</ul>
			</div>
		</div> -->
		
	</div>
</template>


<script>
	export default {
		data() {
			return {
				Router: this.$Router,

				produtList: [{}, {}, {}, {}],
				mainData: {},
				type: '',
				isCollect:false,
				messageData:[]
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			var collectData = self.$Utils.getStorageArray('cartDataTwo');
			
			for (var i = 0; i < collectData.length; i++) {
				if(collectData[i].id==self.id){
					self.isCollect = true
				}
			};
			console.log(self.type)
			self.$Utils.loadAll(['getUserInfoData','getMessageData'], self);
		},

		methods: {
			
			collect() {
				const self = this;	
				if (JSON.stringify(self.mainData) == '{}') {		
					self.$Utils.showToast('商品信息错误', 'none');
					return;
				};
				if(self.isCollect){
					var res = self.$Utils.delStorageArray('cartDataTwo',self.mainData,'id');
					if (res) {
						self.isCollect = false;
						self.$Utils.showToast('取消成功', 'none');
					};
				}else{
					var res = self.$Utils.setStorageArray('cartDataTwo', self.mainData, 'id', 999);
					if (res) {
						self.isCollect = true;
						self.$Utils.showToast('加入成功', 'none');
					};
				}
				
			},

			goBuy() {
				const self = this;
				if (JSON.stringify(self.mainData) == '{}') {
					api.showToast('商品错误', 'none', 1000);
					return;
				};
				var orderList = {
					product: [{
						id: self.mainData.id,
						count: 1,
						product: self.mainData
					}],
					type:self.type,
				};
				
				if(self.type!=3){
					uni.setStorageSync('payPro', orderList);
					self.$Router.navigateTo({
						route: {
							path: '/pages/confirmOrder/confirmOrder?type='+self.type
						}
					})
				}else{
					uni.setStorageSync('order', orderList);
					self.$Router.navigateTo({
						route: {
							path: '/pages/yuyue/yuyue'
						}
					})
				}
				
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.getMainData();
				};
				self.$apis.userInfoGet(postData, callback);
			},


			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					id: self.id,
					thirdapp_id:3
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData(isNew) {
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				postData.searchItem = {
					thirdapp_id: 3,
					relation_id :self.id,
					type:2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
					}
					console.log('23',res.info.total)
					self.totalMessage = res.info.total;
					console.log('self.messageData', self.messageData)
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},

		}
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/numCar.css";
	@import "../../assets/style/prodctDetail.css";
	page{padding-bottom: 60px;}
</style>

