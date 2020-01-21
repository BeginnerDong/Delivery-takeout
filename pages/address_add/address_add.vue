<template>
	<div>
		<div class="addressAdd fs13">
			<ul>
				<li>
					<div class="title">联系人：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入真实姓名" v-model="submitData.name">
					</div>
				</li>
				<li>
					<div class="title">&nbsp;</div>
					<div class="rightMsg flex sexSelt">
						<div class="flexRowBetween sexSelt" style="width: 100px;">
							<span class="flex" @click="change('1')"><image class="icon" 
							:src="submitData.country=='1'?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'" />男</span>
							<span class="flex" @click="change('2')"><image class="icon" 
							:src="submitData.country=='2'?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'"/>女</span>
						</div>
					</div>
				</li>
				<li>
					<div class="title">手机号码：</div>
					<div class="rightMsg">
						<input type="number" placeholder="请输入手机号码"  maxlength="11" v-model="submitData.phone">
					</div>
				</li>
				<li>
					<div class="title">选择地区：</div>
					<div class="rightMsg flexRowBetween color9" @click="chooseAddress()">
						<input type="text" placeholder="选择您的位置" disabled="true"   v-model="submitData.city"><img class="arrowR" src="../../static/images/icon.png" alt="">
					</div>
				</li>
				<li>
					<div class="title">详细地址：</div>
					<div class="rightMsg">
						<input type="text" placeholder="请输入详细地址" v-model="submitData.detail">
					</div>
				</li>
				<li>
					<div class="title" style="width: 50%;">设为默认地址</div>
					<div class="rightMsg" style="width:50%;text-align: right">
						<switch @change="choose" :checked="submitData.isdefault==1?true:false" color="#2FA0ED"></switch>
						<!-- <img v-if="!is_show" class="mmIcon" src="../../static/images/address-icon03.png" alt="">
						<img v-if="is_show" class="mmIcon" src="../../static/images/address-icon02.png" alt=""> -->
					</div>
				</li>	
			</ul>
		</div>
		
		<div class="submitbtn" style="margin-top:50px;">
			<a class="btn" @click="Utils.stopMultiClick(submit)">保存地址</a>
		</div>
	</div>
</template>


<script>
	import mpvuePicker from '../../components/mpvue-picker/mpvuePicker.vue';
	// https://github.com/zhetengbiji/mpvue-citypicker
	import mpvueCityPicker from '../../components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '../../common/city.data.js';

	export default {
		components: {
			mpvuePicker,
			mpvueCityPicker
		},
		
		
		data() {
			return {
				submitData: {
					name: '',
					city:'',
					detail: '',
					phone:'',
					longitude:'',
					latitude:'',
					isdefault:1,
					country:1
				},
				
				mulLinkageTwoPicker: cityData,
				cityPickerValueDefault: [0, 0, 0],
				themeColor: '#F98A48',
				Utils:this.$Utils,
				mode: '',
				deepLength: 1,
				pickerValueDefault: [0],
				pickerValueArray:[],
				
			}
		},
		onLoad(options) {
			const self = this;
			if(options.id){
				self.id = options.id;
				var res = self.$Token.getProjectToken(function(){
					self.$Utils.loadAll(['getMainData'], self)
				});
				if(res){
					self.$Utils.loadAll(['getMainData'], self)
				};
			}
		},
		
		methods: {
			
			change(country){
				const self = this;
				console.log(2324,country)
				if(self.submitData.country!=country){
					console.log(2324,country)
					self.submitData.country = country
				}
			},
			
			chooseAddress(e) {
				const self = this;
				uni.authorize({
				    scope: 'scope.userLocation',
				    success() {
				        uni.chooseLocation({
				        	success: (res) => {
				        		console.log(res)
				        		self.submitData.city = res.name;
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
				        							self.submitData.city = res.name;
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
			
			
			
			
			
			
			choose(e) {
				const self = this;
				if(e.target.value){
					self.submitData.isdefault = 1
				}else{
					self.submitData.isdefault = 0
				}
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
			},

			getMainData(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					console.log(res);
					
					self.submitData.name = res.info.data[0].name;
					self.submitData.detail = res.info.data[0].detail;
					self.submitData.city = res.info.data[0].city;
					self.submitData.phone = res.info.data[0].phone;
					self.submitData.isdefault = res.info.data[0].isdefault;
					self.submitData.country = res.info.data[0].country;
					self.submitData.longitude = res.info.data[0].longitude;
					self.submitData.latitude  = res.info.data[0].latitude;
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},



			addressUpdate() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = self.id;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('修改成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						self.$Utils.showToast(data.msg,'error')
					};
					
				};
				self.$apis.addressUpdate(postData, callback);
			},


			addressAdd() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);

				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
					
				};
				self.$apis.addressAdd(postData, callback);
			},


			submit() {
				const self = this;
				
				var phone = self.submitData.phone;
				const pass = self.$Utils.checkComplete(self.submitData);

				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					
					if (self.id) {

						self.addressUpdate();
					} else {

						self.addressAdd();
					}

			
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},

		}
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/address.css";

</style>

