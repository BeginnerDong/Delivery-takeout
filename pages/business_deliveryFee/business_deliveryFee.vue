<template>
	<div>
		
		<div class="myExtendTop white center">
			<div class="yuan pdb10">今天总共15单</div>
			<div class="bigNum" style="font-size: 32px;">566.9<em class="fs12">元</em></div>
			
		</div>
		
		<div class="jifenTwoNum flexRowBetween boxShaow radius5">
			<div class="item">
				<p class="pdb5 flexCenter fs13 color2">开始时间<img class="arrow" src="../../static/images/icon2.png" ></p>
				<div class="num color6 fs13">2019.9.23</div>
			</div>
			<div class="item">
				<p class="pdb5 flexCenter fs13 color2">结束时间<img class="arrow" src="../../static/images/icon2.png" ></p>
				<div class="num color6 fs13">2019.10.23</div>
			</div>
		</div>
		
		<div class="orderList pdlr4">
			<ul>
				<li class="bordB1" v-for="(item,index) in dlvrFreeDate" :key="index">
					<div class="infor">
						<div class="datt flexRowBetween">
							<div class="left color6">订单编号：123356885555</div>
							<div class="price">5.2</div>
						</div>
						<div class="msg mglr10 pr radius5 fs12">
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">韩国泡菜</span>
								<span class="num">×1</span>
								<span class="mny">￥21.0</span>
							</p>
							<p class="child  flexRowBetween">
								<span class="tit avoidOverflow">牛肉土豆饼</span>
								<span class="num">×1</span>
								<span class="mny">￥4.0</span>
							</p>
						</div>
					</div>
				</li>
			</ul>
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
				dlvrFreeDate:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/user.css";
	@import "../../assets/style/order.css";
	
	.orderList li{box-shadow: initial; margin-top: 0;}
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4% 60px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; line-height:40px;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 15px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .right{width: 50%;}
</style>

