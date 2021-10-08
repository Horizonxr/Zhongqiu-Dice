<template>
	<view class="bg">
		<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/3da9c9f4-a982-4085-bd3c-3d31a9fa2358.png" mode=""></image>
		<view class="title">
			<text>创建房间</text>
		</view>
		<view class="player_table">
			<view class="player_table_head">
				<view>序号</view>
				<view>玩家名称</view>
				<view>操作</view>
			</view>
			<view class="player_content" v-for="(item,key) in playerInfo" :key="item.key">
				<view>{{key+1}}</view>
				<view>
					<input class="player_name_input" v-model="playerInfo[key].name" placeholder="请输入玩家名称" />
				</view>
				<view class="iconfont icon-shanchu" @click="deletePlayer(key)"></view>
			</view>
			<view class="add_player_text" @click="add_player()">点我新增玩家</view>
			<uni-popup ref="popup" type="top">
				已达到人数上限！
			</uni-popup>
		</view>
		<view class="bottom">
			<view @click="toIndex()">返回</view>
			<view @click="toGame()">确认</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				player_num_now:1,
				playerInfo:[
					{
						id:0,
						name:""
					}
				]
			}
		},
		methods: {
			toIndex(){
				uni.navigateBack()
			},
			toGame(){
				uni.setStorage({
				    key: 'player_info',
				    data: this.playerInfo,
				    success: function () {
				        console.log('success');
				    }
				});
				uni.setStorage({
				    key: 'player_num',
				    data: this.player_num_now,
				    success: function () {
				        console.log('success');
				    }
				});
				uni.redirectTo({
					url:"../game/game"
				})
			},
			add_player(){
				if (this.player_num_now >=8){
					this.$refs.popup.open('center')
					return
				}
				this.playerInfo.push({
					id:this.player_num_now,
					name:""
				})
				this.player_num_now++
			},
			deletePlayer(id){
				this.playerInfo.splice(id,1)
				this.player_num_now--
				for (let i = 0;i<this.playerInfo.length;i++){
					this.playerInfo[i].id = i
				}
			}
		}
	}
</script>

<style lang="scss">
	.bg{
		margin: 0;
		padding: 0;
		width: 100vw;
		height: 100vh;
		position: relative;
		image{
			position: absolute;
			width: 100vw;
			height: 100vh;
			z-index: -999;
		}
		.title{
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 130rpx;
			font-weight: 700;
			color: $create-room-text-color;
			width: 100%;
			height: 200rpx;
			text-align: center;
			top:0;
			z-index: 1;
		}
		.player_table{
			position: absolute;
			height: 600rpx;
			width: 100%;
			color: $bg-color;
			display: flex;
			flex-direction: column;
			justify-content: top;
			align-items: center;
			.player_table_head{
				font-size: 60rpx;
				font-weight: 700;
				width: 90%;
				margin-bottom: 20rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				text-align: center;
				view:nth-child(1){
					width: 20%;
				}
				view:nth-child(2){
					width: 60%;
				}
				view:nth-child(3){
					width: 20%;
				}
			}
			.player_content{
				font-size: 40rpx;
				width: 80%;
				display: flex;
				justify-content: center;
				align-items: center;
				text-align: center;
				view:nth-child(1){
					width: 20%;
				}
				view:nth-child(2){
					width: 60%;
				}
				view:nth-child(3){
					font-size: 50rpx;
					width: 20%;
				}
			}
			.add_player_text{
				margin-top: 30rpx;
				padding: 8rpx;
				font-size: 30rpx;
				font-weight: 700;
				border: 5rpx solid $bg-color;
				border-radius: 10rpx;
			}
			uni-popup{
				color: white;
				font-size: 80rpx;
			}
		}
		.bottom{
			display: flex;
			justify-content: center;
			align-items: center;
			position: absolute;
			bottom: 0;
			height: 300rpx;
			width: 100%;
			z-index: 1;
			view{
				width: 50%;
				font-size: 70rpx;
				text-align: center;
				font-weight: 700;
				color: $icon-color;
				z-index: 2;
			}
		}
	}

</style>
