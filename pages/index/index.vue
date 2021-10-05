<template>
	<view class="start_bg">
		<image class="bg_image" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/94aeee79-e979-4c31-aa58-374598f4e3cb.jpg"></image>
		<image class="start_title" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/cc10bd7d-650e-44a5-ae8c-e50057441b14.png"></image>
		<image class="single_player" @click="to_single_player()" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/733de78c-14ee-4c8b-a971-3fc7d87cc0e6.png"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			to_single_player(){
				uni.navigateTo({
					url:"../game/game"
				})
			}
		}
	}
</script>

<style lang="scss">
	.start_bg{
		width: 100%;
		height: 1600rpx;
		margin: 0;
		padding: 0;
		position: relative;
		.bg_image{
			position: absolute;
			width: 100%;
			height: 1600rpx;
			z-index: -999;
		}
		.start_title{
			position: relative;
			width: 636rpx;
			height: 200rpx;
			top: -23rpx;
			left: 56rpx;
		}
		.single_player{
			position: relative;
			width: 300rpx;
			height: 76rpx;
			top:900rpx;
			left: 224rpx;
		}
		
	}
</style>
