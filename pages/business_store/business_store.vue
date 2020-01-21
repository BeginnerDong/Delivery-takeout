<template>
	<div>
		
		<div class="storeGl">
			<ul>
				<li class="item flexRowBetween">
					<div class="ll title fs13">店铺名称</div>
					<div class="rr flexEnd">
						<input type="text"  v-model="submitData.name" placeholder="请输入店铺名称" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">营业时间</div>
					<div class="rr flexEnd">
						<input type="text"  v-model="submitData.passage1" placeholder="请输入营业时间" />
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">联系电话</div>
					<div class="rr flexEnd">
						<input type="number"   placeholder="请输入联系电话" v-model="submitData.phone"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">店铺地址</div>
					<div class="rr flexEnd" @click="chooseAddress">
						<input type="text" disabled="true"   placeholder="请输入店铺地址" v-model="submitData.address"/>
					</div>
				</li>
				<li class="item">
					<div class="title fs13">店铺主图</div>
					<div class="flex uploadImg">
						<div class="lis" v-for="item in submitData.mainImg">
							<img :src="item.url" >
						</div>
						<div class="lis pr" @click="upLoadImg()" v-if="submitData.mainImg&&submitData.mainImg.length<5">
							<img src="../../static/images/merchants-icon10.png" >
							<!-- <input class="upBtn" type="file"   placeholder="" /> -->
						</div>
					</div>
					<p class="fs12 color9 pdt10">可传1~5张</p>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">愿意承担配送费用</div>
					<div class="rr flexEnd">
						<input type="text"   placeholder="请输入承担配送费用" v-model="custom_rule.delivery_reduce"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最低下单金额</div>
					<div class="rr flexEnd">
						<input type="text"   placeholder="请输入最低下单金额" v-model="custom_rule.limit_price"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最远配送范围</div>
					<div class="rr flexEnd">
						<input type="text"   placeholder="请输入最远配送范围" v-model="custom_rule.limit_distance"/>
					</div>
				</li>
				<li class="item flexRowBetween" style="align-items: initial;">
					<div class="ll title fs13">设置满减</div>
					<div class="rr flexColumn manjian fs12 color9">
						<div class="w flexEnd" v-for="item in param">满
						<input type="text" v-model="item.standard">元，减
						<input type="text" v-model="item.value">元</div>
					</div>
				</li>
				
			</ul>
		</div>
		
		<div class="submitbtn mgt40">
			<button class="btn" type="submit" @click="submit">确定</button>
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
				submitData:{
					name:'',
					passage1:'',
					phone:'',
					address:'',
					mainImg:[],
					longitude:'',
					latitude:''
				},
				custom_rule:{
					
				},
				param:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData','thirdAppGet'], self);
		},
		
		methods: {
			
			chooseAddress(e) {
				const self = this;
				uni.authorize({
				    scope: 'scope.userLocation',
				    success() {
				        uni.chooseLocation({
				        	success: (res) => {
				        		console.log(res)
				        		self.submitData.address = res.address;
				        		self.submitData.longitude = res.longitude;
				        		self.submitData.latitude  = res.latitude;
				        	},
				        	fail: (e) => {
				        		uni.getSetting({
				        			success: (res) => {
				        				console.log(res)
				        				let locaAuth = res.authSetting['scope.userLocation']
				        				if (locaAuth) {/* 判断位置是否已经授权，是选择地图位置点击取消触发的fail，再选择位置 */
				        					console.log('地图点击取消')
				        					uni.chooseLocation({
				        						success: (res) => {
				        							self.submitData.address = res.address;
				        							self.submitData.longitude = res.longitude;
				        							self.submitData.latitude  = res.latitude;
				        						},
				        					});
				        				}
				        				if (!locaAuth) { /* 如果地理位置没授权 */
				        					console.log(222)
				        					uni.showModal({
				        					    title: '提示',
				        					    content: '需要授权位置信息',
				        						confirmColor:'#ca1c1d',
				        						showCancel:true,
				        					    success: function (res) {
				        					        if (res.confirm) {
				        					            uni.openSetting({
				        					            	success: (res) => {
				        					            		console.log(res.authSetting)
				        					            	},
				        					            	fail: (res) => {
				        					            		console.log(res)
				        					            	},
				        					            });
				        					        } else if (res.cancel) {
				        					           
				        					        }
				        					    }
				        					});			
				        					
				        				
				        				}
				        			}
				        		})
				        	}
				        });
				    },
					fail: (e) => {
						uni.showModal({
						    title: '提示',
						    content: '需要授权位置信息',
							confirmColor:'#ca1c1d',
							showCancel:true,
						    success: function (res) {
						        if (res.confirm) {
						            uni.openSetting({
						            	success: (res) => {
						            		console.log(res.authSetting)
						            	},
						            	fail: (res) => {
						            		console.log(res)
						            	},
						            });
						        } else if (res.cancel) {
						           
						        }
						    }
						});
					}
				})
				
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
				};
				postData.getAfter = {
					param6:{
						tableName:'Param',
						middleKey:'status',
						key:'status',
						searchItem:{
							type:5,
							fee_type:6,
							is_use:1
						},
						condition:'=',
						order:{
							standard:'asc'
						}
					},
				};
				postData.tokenFuncName = 'getStoreToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.param = self.mainData.param6;
						self.submitData.name = self.mainData.name;
						self.submitData.phone = self.mainData.phone;
						self.submitData.address = self.mainData.address;
						self.submitData.passage1 = self.mainData.passage1;
						self.submitData.mainImg = self.mainData.mainImg;
						self.submitData.latitude = self.mainData.latitude;
						self.submitData.longitude = self.mainData.longitude;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			thirdAppGet() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:3,
				};
				postData.tokenFuncName = 'getStoreToken';
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.thirdAppData = res.info.data[0];
						self.custom_rule = self.thirdAppData.custom_rule
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('thirdAppGet');
				};
				self.$apis.thirdAppGet(postData, callback);
			},
			
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				const passTwo = self.$Utils.checkComplete(self.submitDataTwo);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass&&passTwo) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getStoreToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('storeInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [
					{
						tableName: 'ThirdApp',
						FuncName: 'update',
						data: {
							custom_rule:self.$Utils.cloneForm(self.custom_rule)
						}
					}
				];	
				for (var i = 0; i < self.param.length; i++) {
					postData.saveAfter.push(
						{
							tableName: 'Param',
							FuncName: 'update',
							data: self.param[i],
							searchItem:{
								id:self.param[i].id
							}
						}
					)
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('修改成功', 'none');
						
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData.mainImg.push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 5,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						
						for (var i = 0; i < tempFilePaths.length; i++) {
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getStoreToken'
							}, callback)
						}
						
						
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/storeGl.css";
	
</style>

