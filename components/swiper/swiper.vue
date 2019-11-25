<template>
	<view class="banner-box pdlr4">
		<view class="banner">
			<swiper class="swiper-box" indicator-dots="indicatorDots" autoplay="autoplay" interval="interval" duration="duration" indicator-active-color="#FFCB1E">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			list: Array
		},
		data() {
			return {
				currIndex: 0,
				Router:this.$Router,
				labelData: [
					"../../static/images/home-banenr.png",
					"../../static/images/details-img1.png",
				]
			};
		},
		methods: {
			change(s) {
				this.currIndex = s.detail.current;
			}
		}
	}
</script>
<style>
	.swiper-box {
		height: 360rpx;
		box-sizing: border-box;
		overflow: hidden;
		border-radius:10rpx;
	}
	
	.swiper-box swiper-item {
		width: 100%;
	}
	
	.swiper-box image {
		width: 100%;
		height: 100%;
	}
</style>

