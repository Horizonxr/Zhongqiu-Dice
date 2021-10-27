<template>
	<view class="start_bg">
		<image class="bg_image" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/354042ae-5f62-41cb-b278-1c02ece0beda.png"></image>
		<image class="start_title" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/cc10bd7d-650e-44a5-ae8c-e50057441b14.png"></image>
		<view class="options">
			<image class="single_player" @click="to_single_player()" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/733de78c-14ee-4c8b-a971-3fc7d87cc0e6.png"></image>
			<image class="create_room" @click="to_create_room()" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/4a58853b-1ff6-491a-a496-7bc293cefd1d.png" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				round:10,
				player_num:1,
				playerInfo:[
					{
						id:0,
						name:"玩家",
						avatar_url:"",
						player_award_history:[0,0,0,0,0,0,0]
					}
				]
			}
		},
		onLoad() {

		},
		methods: {
			getUserProfile() {
			    let that = this
			    uni.getUserProfile({
			        desc: "用于完善用户信息",
			        success: (res1) => {
			            that.info = res1.userInfo;
			            console.log(res1)
			            uni.showToast({
			                icon:"none",
			                title:'获取成功'
			            })
			        },
			        fail: (err) => {
			            console.log(err)
			            uni.showToast({
			                icon:"none",
			                title:'用户拒绝获取'
			            })
			        }  
			    })
			},
			to_single_player(){
				// this.$options.methods.getUserProfile.bind(this)()
				let that = this
				uni.getUserProfile({
				    desc: "用于完善用户信息",
				    success: (res) => {
				        console.log(res)
						this.playerInfo[0].name = res.userInfo.nickName
						this.playerInfo[0].avatar_url = res.userInfo.avatarUrl
				        uni.showToast({
				            icon:"none",
				            title:'获取成功'
				        })
						uni.setStorage({
						    key: 'player_info',
						    data: this.playerInfo,
						    success: function () {
						        console.log('success');
						    }
						});
						uni.setStorage({
						    key: 'player_num',
						    data: this.player_num,
						    success: function () {
						        console.log('success');
						    }
						});
						uni.setStorage({
						    key: 'round',
						    data: this.round,
						    success: function () {
						        console.log('success');
						    }
						});
						uni.navigateTo({
							url:"../game/game"
						})
				    },
				    fail: (err) => {
				        console.log(err)
				        uni.showToast({
				            icon:"none",
				            title:'用户拒绝获取'
				        })
				    }  
				})
			},
			to_create_room(){
				uni.navigateTo({
					url:"../creatRoom/creatRoom"
				})
			}
		}
	}
</script>

<style lang="scss">
	.start_bg{
		width: 100%;
		height: 100vh;
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
		.options{
			position: absolute;
			bottom: 0;
			width: 100%;
			height: 30vh;
			.single_player{
				position: absolute;
				width: 300rpx;
				height: 76rpx;
				top:0rpx;
				left: 224rpx;
			}
			.create_room{
				position: relative;
				width: 346rpx;
				height: 110rpx;
				top:200rpx;
				left: 202rpx;
			}
		}
	}
</style>
