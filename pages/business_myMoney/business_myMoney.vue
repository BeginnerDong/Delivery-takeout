<template>
	<div>
		
		<div class="myExtendTop white center">
			<div class="bigNum mgb5"><em class="fs12">￥</em>5623</div>
			<div class="yuan">总收益</div>
		</div>
		
		<div class="jifenTwoNum flexRowBetween boxShaow radius5">
			<div class="item">
				<p class="pdb5 flexCenter">
					5月<img class="arrow" src="../../static/images/icon2.png" >
				</p>
				<div class="num price">2000</div>
			</div>
			<div class="item">
				<p class="pdb5  flexCenter">21日<img class="arrow" src="../../static/images/icon2.png" ></p>
				<div class="num price">2000</div>
			</div>
		</div>
		
		
		<div class="myRowBetween" >
			<div class="item flexRowBetween fs13 color6" >
				<div class="left ">2019年10月23号</div>
				<div class="right flexEnd">￥562</div>
			</div>
			<div class="item flexRowBetween fs13 color6" >
				<div class="left">2019年10月23号</div>
				<div class="right flexEnd">￥562</div>
			</div>
			<div class="item flexRowBetween fs13 color6" >
				<div class="left ">2019年10月23号</div>
				<div class="right flexEnd">￥562</div>
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
	@import "../../assets/style/user.css";
	/* 我的收益 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 20px 4% 60px 4%; background: #2FA0ED;}
	.myExtendTop .bigNum{font-size: 40px; line-height:40px;}
	.myExtendTop .txBtn{ width: 100px; height: 30px;line-height: 30px; background: #fff; color: #333; border-radius: 3px; margin: 0 auto;font-size: 13px; display: block;}
	
	/* 信息RowBetween */
	.myRowBetween .item{ padding: 15px 4%;border-bottom: 1px solid #eee;background: #fff;}
	.myRowBetween .item .left{width: 50%;}
	.myRowBetween .item .right{width: 50%;}
</style>

