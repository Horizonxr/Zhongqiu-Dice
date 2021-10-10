<template>
	<view class="bg">
		<uni-popup ref="avatar_select_popup" type="dialog">
		    <view class="choose_avatar_wrapper">
				<view class="iconfont icon-fanhui" @click="this.$refs.avatar_select_popup.close()"></view>
				<view class="choose_avatar_title">请选择你的头像</view>
				<view class="choose_avatar_image">
					<view v-for="(item, key) in avatar_list" :key="item.key"  @click="chooseAvator(key)">
						<image :src="item.avatar_url" :style="{'opacity': item.chosen === 0 ? 0.3 : 1 , 'border': key === playerInfo[change_avatar_id].avatar_id ? '6rpx solid white' : ''}"></image>
					</view>
				</view>
			</view>
		</uni-popup>
		<uni-popup class="error_message" ref="error_popup" type="message">
			<uni-popup-message type="warn" message="失败消息" :duration="2000">
				<view>{{error_message}}</view>
			</uni-popup-message>
		</uni-popup>
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
				<view class="player_options">
					<view class="iconfont icon-shanchu" @click="deletePlayer(key)"></view>
					<view class="iconfont icon-edit" @click="editPlayerAvatar(key)"></view>
				</view>
			</view>
			<view class="add_player_text" @click="add_player()">点我新增玩家</view>
			<uni-popup ref="popup" type="top">
				已达到人数上限
			</uni-popup>
		</view>
		<view class="round">
			<view>游戏轮数:</view>
			<view>
				<input class="round_input" v-model="round" placeholder="请输入游戏轮数" placeholder-class="round_placeholder"/>
			</view>
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
				error_message:"",
				change_avatar_id:-1,
				avatar_list:[
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/da8610d8-2c2c-4710-be53-3e9d60ba11d7.jpeg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/bef6621e-e4df-4297-a6a3-53b7383132f3.jpg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/0855e1cf-b2cb-46cf-a576-0bf6bc947e2a.jpeg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/b8c916ac-18cc-479d-86f7-f85951dfcc27.jpeg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/6ecebc46-46bf-4991-a849-c5ab0b7c2387.jpeg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/452ac5e0-ba1d-4aeb-90f4-45d4e87e2385.jpg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/2fb73123-fc13-4ea5-9acc-b6dee8cb5d57.jpeg",
						chosen:1
					},
					{
						avatar_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/49ad2222-ab5e-4214-aa33-b6b19a2b40ad.jpg",
						chosen:1
					},
				],
				round:1,
				player_num_now:1,
				playerInfo:[
					{
						id:0,
						name:"",
						avatar_url:"",
						avatar_id:-1,
						player_award_history:[0,0,0,0,0,0,0]
					}
				]
			}
		},
		methods: {
			toIndex(){
				uni.navigateBack()
			},
			toGame(){
				if (!this.$options.methods.check_info.bind(this)()){
					return
				}
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
				uni.setStorage({
				    key: 'round',
				    data: this.round,
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
					name:"",
					avatar_url:"",
					avatar_id:-1,
					player_award_history:[0,0,0,0,0,0,0]
				})
				this.player_num_now++
			},
			deletePlayer(id){
				if (this.playerInfo.length == 1){
					this.error_message = "至少剩余一名玩家"
					this.$refs.error_popup.open('top')
					return
				}
				if (this.playerInfo[id].avatar_id!==-1){
					this.avatar_list[this.playerInfo[id].avatar_id].chosen = 1
				}
				this.playerInfo.splice(id,1)
				this.player_num_now--
				for (let i = 0;i<this.playerInfo.length;i++){
					this.playerInfo[i].id = i
				}
				console.log("释放了头像"+this.playerInfo[id].avatar_id)
				console.log("玩家ID是"+id)
			},
			editPlayerAvatar(id){
				this.change_avatar_id = id
				this.$refs.avatar_select_popup.open('top')
				console.log("想要改变的账号"+id)
			},
			chooseAvator(id){
				if (this.avatar_list[id].chosen !== 0){
					if (this.playerInfo[this.change_avatar_id].avatar_id!==-1){
						let pre_id = this.playerInfo[this.change_avatar_id].avatar_id
						this.avatar_list[pre_id].chosen = 1
					}
					this.playerInfo[this.change_avatar_id].avatar_id = id
					this.playerInfo[this.change_avatar_id].avatar_url = this.avatar_list[id].avatar_url
					this.avatar_list[id].chosen = 0
					this.$refs.avatar_select_popup.close()
				}
				else {
					this.error_message = "头像已经被选择，请换一个吧"
					this.$refs.error_popup.open('top')
					console.log("头像已经被选择"+id)
				}
			},
			check_info(){
				for (let i = 0;i<this.playerInfo.length;i++){
					let pl = this.playerInfo[i]
					if (pl.avatar_id === -1 || pl.name === ""){
						this.error_message = "请检查玩家名称，头像是否填写"
						this.$refs.error_popup.open('top')
						return 0
					}
				}
				return 1
			}
		}
	}
</script>

<style lang="scss">
	.bg{
		.choose_avatar_wrapper{
			width: 100%;
			height: 100vh;
			.icon-fanhui{
				width: 100%;
				height: 10vh;
				font-size: 70rpx;
				padding: 20rpx;
				color:white;
			}
			.choose_avatar_title{
				height: 10vh;
				width: 100%;
				color: white;
				font-size: 100rpx;
				font-weight: 700;
				text-align: center;
				margin-bottom: 100rpx;
			}
			.choose_avatar_image{
				display: flex;
				justify-content: center;
				flex-direction: row;
				flex-wrap: wrap;
				width: 100%;
				height: 40vh;
				view{
					height: 80rpx;
					width: 25%;
					display: flex;
					justify-content: center;
					align-items: center;
					image{
						width: 100rpx;
						height: 100rpx;
						border-radius: 50%;
					}
				}
			}
		}
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
				width: 85%;
				display: flex;
				justify-content: center;
				align-items: center;
				text-align: center;
				view:nth-child(1){
					width: 20%;
				}
				view:nth-child(2){
					width: 80%;
				}
				.player_options{
					width: 20%;
					display: flex;
					view{
						width: 50%;
						font-size: 50rpx;
					}
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
		.round{
			position: absolute;
			left:50%;
			transform: translateX(-50%);
			width: 100%;
			bottom: 160rpx;
			display: flex;
			text-align: center;
			view:nth-child(1){
				width: 50%;
				font-size: 70rpx;
				font-weight: 700;
				color: $icon-color;
				z-index: 2;
			}
			view:nth-child(2){
				width: 50%;
				font-size: 70rpx;
				font-weight: 700;
				color: $icon-color;
				z-index: 2;
				input{
					height: 90rpx;
				}
				.round_placeholder{
					font-size: 50rpx;
				}
			}
		}
		.bottom{
			display: flex;
			justify-content: center;
			align-items: center;
			position: absolute;
			bottom: 30rpx;
			height: 100rpx;
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
