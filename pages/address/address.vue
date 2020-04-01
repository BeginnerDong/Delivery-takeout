<template>
	<div>
		
		<div class="addressLis fs13 color6 pdlr4">
			<ul>
				<li class="bordB1" v-for="(item,index) in mainData" :key="index">
					<div class="flexRowBetween" @click="choose(index)">
						<div class="" style="width: 88%;">
							<p class="adrs color3">{{item.city+item.detail}}</p>
							 <p class="flex fs13 color9 name">
								<span>{{item.name}}</span>
								<span class="mgl10 mgr20">{{item.country==1?'先生':'女士'}}</span>
								<span>{{item.phone}}</span>
							</p>
						</div>
						<div class="right fs12 flexEnd" style="width: 10%;">
							<div class="tt"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/address_add/address_add?id='+$event.currentTarget.dataset.id}})">
								<img src="../../static/images/address-icon1.png" alt="" />
							</div>
						</div>
					</div>
					<div>
						<div class="flex fs12 color9" :data-id="item.id" @click="updateAddress($event.currentTarget.dataset.id)">
						 	<image class="xing" :src="item.isdefault==1?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'"/>
							<span>默认地址</span>
						</div>
					</div>
					
				</li>
			</ul>
		</div>
		
		<div class="submitbtn" style="margin-top: 80px;position: fixed;">
			<a class="btn" style="position: fixed;margin: 0;bottom: 0;width: 100%;border-radius: 0;"   @click="Router.redirectTo({route:{path:'/pages/address_add/address_add?name='+name}})">添加地址</a>
		</div>
	</div>
</template>


<script>
	export default {

		data() {
			return {
				mainData: [],
				Router:this.$Router,
				choosedIndex: -1
			}
		},

		onLoad(options) {
			const self = this;
			
		},

		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onShow() {
			const self = this;
			self.mainData = [];
			var res = self.$Token.getProjectToken(function() {
				self.$Utils.loadAll(['getMainData'], self)
			});
			if (res) {
				self.$Utils.loadAll(['getMainData'], self)
			};
		},

		methods: {

			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta:1
				})
			},

			getMainData() {
				const self = this;

				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},





			deleteAddress(id) {
				const self = this;
				const postData = {};
				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressDelete(postData, callback)
			},


			updateAddress(id) {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.data = {
					isdefault: 1
				}
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressUpdate(postData, callback);
			},




		}
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/address.css";

</style>

