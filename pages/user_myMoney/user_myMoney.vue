<template>
	<div>
		
		<div class="myExtendTop white">
			<div class="bigNum">5623</div>
			<div class="yuan">收益(元)</div>
			<a class="txBtn"  @click="Router.navigateTo({route:{path:'/pages/user_myCashOut/user_myCashOut'}})">提现</a>
		</div>
		
		<div class="myRowBetween" >
			<div class="item flexRowBetween" >
				<div class="left">
					<div class="avoidOverflow">提现</div>
					<div class="time">2019-09-19</div>
				</div>
				<div class="right pubColor">-223</div>
			</div>
			<div class="item flexRowBetween" >
				<div class="left">
					<div class="avoidOverflow">提现</div>
					<div class="time">2019-09-19</div>
				</div>
				<div class="right pubColor">-235</div>
			</div>
			<div class="item flexRowBetween" >
				<div class="left">
					<div class="avoidOverflow">收益收益</div>
					<div class="time">2019-09-19</div>
				</div>
				<div class="right red">+236</div>
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
				mainData: []
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
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; padding: 0 4%; line-height:44px;}
	.myExtendTop .yuan{ font-size:14px; padding:5px 0 15px 0;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 10px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .time{color: #999; font-size: 12px;}
	.myRowBetween .item .left .time{margin-top: 5px;}
	.myRowBetween .item .right{width: 50%;text-align: right;font-size: 15px; display: flex; justify-content: flex-end;align-items: center;}

</style>

