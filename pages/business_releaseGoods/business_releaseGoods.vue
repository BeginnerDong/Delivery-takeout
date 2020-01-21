<template>
	<div>
		
		<div class="storeGl">
			<ul>
				<li class="item flexRowBetween">
					<div class="ll title fs13">商品类别：</div>
					<div class="rr flexEnd">
						<picker @change="bindPickerChange" :value="index" :range="labelData" range-key="title">
							<div style="color: #999;" class="uni-input">{{labelData[index]&&labelData[index].title?labelData[index].title:'请选择'}}</div>
						</picker>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">商品名称</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入商品名称" v-model="submitData.title"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">商品价格</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入商品价格" v-model="submitData.o_price"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">折扣</div>
					<div class="rr flexEnd">
						<input type="number" name="" value="" placeholder="请输入商品折扣" v-model="submitData.discount"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">最终价格</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="56" placeholder="请输入最终价格" v-model="submitData.price"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">包装费</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入包装费"  v-model="submitData.packing_price"/>
					</div>
				</li>
				<li class="item flexRowBetween">
					<div class="ll title fs13">库存</div>
					<div class="rr flexEnd">
						<input type="text" name="" value="" placeholder="请输入库存" v-model="submitData.stock" />
					</div>
				</li>

				<li class="item">
					<div class="title fs13">商品图片</div>
					<div class="flex uploadImg">
						<div class="lis" v-for="item in submitData.mainImg">
							<img :src="item.url" >
						</div>
						<div class="lis pr" @click="upLoadImg('1')" v-if="submitData.mainImg&&submitData.mainImg.length<1">
							<img src="../../static/images/merchants-icon10.png" >
							<!-- <input class="upBtn" type="file" name="" value="" placeholder="" /> -->
						</div>
						
					</div>
				</li>
				<li class="item">
					<div class="title fs13">商品详情图片</div>
					<div class="flex uploadImg">
						<div class="lis" v-for="item in submitData.bannerImg">
							<img :src="item.url" >
						</div>
						<div class="lis pr"  @click="upLoadImg('2')">
							<img src="../../static/images/merchants-icon10.png" >
							<!-- <input class="upBtn" type="file" name="" value="" placeholder="" /> -->
						</div>
						
					</div>
				</li>
				<li class="item flexRowBetween" style="align-items: initial;">
					<div class="ll title fs13" style="width:20%">商品描述</div>
					<div class="rr flexEnd" style="width:80%">
						<textarea rows="" cols="" placeholder="请输入商品描述" value="" v-model="submitData.description"/>
					</div>
				</li>
				
			</ul>
		</div>
		
		<div class="submitbtn mgt40">
			<button class="btn" type="submit" @click="submit()">确定</button>
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
				labelData:[],
				submitData:{
					title:'',
					category_id:'',
					price:'',
					o_price:'',
					packing_price:'',
					mainImg:[],
					bannerImg:[],
					description:'',
					stock:'',
					discount:''
				},
				index:-1
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		methods: {
			
			upLoadImg(type) {
				const self = this;			
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						if(type==1){
							self.submitData.mainImg.push({url:res.info.url,type:'image'})
							console.log(self.submitData)
						}else{
							self.submitData.bannerImg.push({url:res.info.url,type:'image'})
							console.log(self.submitData)
						}
						
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: type==1?1:9,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						if(type==1){
							self.$Utils.uploadFile(tempFilePaths[0], 'file', {
								tokenFuncName: 'getStoreToken'
							}, callback)
						}else{
							for (var i = 0; i < tempFilePaths.length; i++) {
								self.$Utils.uploadFile(tempFilePaths[i], 'file', {
									tokenFuncName: 'getStoreToken'
								}, callback)
							}
						}
						
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			bindPickerChange(e) {
				const self=this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index = e.target.value;
				self.submitData.category_id=self.labelData[self.index].id
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
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
					}
					console.log('self.labelData',self.labelData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			productAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getStoreToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'success')
					}
				};
				self.$apis.productAdd(postData, callback);
			},
			
			
			submit() {
				const self = this;
				
				var phone = self.submitData.phone;
				const pass = self.$Utils.checkComplete(self.submitData);
			
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					self.productAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/storeGl.css";
	
</style>

