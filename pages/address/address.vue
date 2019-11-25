<template>
	<div>
		
		<div class="addressLis fs13 color6 pdlr4">
			<ul>
				<li class="bordB1" v-for="(item,index) in addressLis" :key="index">
					<div class="flexRowBetween">
						<div class="" style="width: 88%;">
							<p class="adrs color3">陕西省西安市莲湖区土门街道创新路70号</p>
							<p class="flex fs13 color9 name">
								<span>张尧</span>
								<span class="mgl10 mgr20">男士</span>
								<span>15689636398</span>
							</p>
						</div>
						<div class="right fs12 flexEnd" style="width: 10%;">
							<div class="tt"  @click="Router.navigateTo({route:{path:'/pages/address_edit/address_edit'}})">
								<img src="../../static/images/address-icon1.png" alt="" />
							</div>
						</div>
					</div>
					<div>
						<div class="flex fs12 color9" @click="change(index)">
							<image class="xing" :src="num==index?'../../static/images/address-icon.png':'../../static/images/address-icon2.png'"/><span>默认地址</span>
						</div>
					</div>
					
				</li>
			</ul>
		</div>
		
		<div class="submitbtn" style="margin-top: 80px;">
			<button class="btn flexCenter w" @click="Router.navigateTo({route:{path:'/pages/address_add/address_add'}})" style="display: flex;"><image style="width: 15px; height: 15px; display: block;margin-right: 10px;" src="../../static/images/address-icon3.png" />新增收货地址</button>
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
				addressLis:[{},{},{}],
				num:0
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(index){
				const self = this;	
				   self.num = index
			},
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res){
				    console.log('getMainData', res);
				    self.mainData.push.apply(self.mainData,res.info.data);		        
				};
				self.$apis.orderGet(postData, callback);
			}
		},
	}
</script>


<style>
	@import "../../assets/style/public.css";
	@import "../../assets/style/address.css";

</style>

