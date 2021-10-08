<template>
	<view class="bg">
		<uni-popup class="EndPopup_wrapper" ref="EndPopup" type="top">
			<view class="EndPopup_table">
				<view>游戏结束</view>
				<view @click="final_result()">查看榜单</view>
				<view @click="restartGame()">重新开始</view>
				<view @click="quitGame()">结束游戏</view>
			</view>
		</uni-popup>
		<view class="top">
			<view class="iconfont icon-quit" @click="quitGame()"></view>
			<view class="room_info_wrapper">
				<view class="room_info">轮数:{{round_now}}/{{round}}</view>
			</view>
			<view class="iconfont icon-rule" @click="openRule()"></view>
			<uni-popup ref="RulePopup" type="center">
				<view class="rule_text_wrapper">
					<view class="iconfont icon-fanhui" @click="$refs.RulePopup.close()"></view>
					<view class="rule_title">博饼规则</view>
					<view class="rule_text">
						<text>
							状元：六个相同点数>五个相同点数>四个“四点”+两个其他点数
							榜眼（对堂）：博出点位为顺子（1，2，3，4，5，6）
							探花（三红）：同时出现三个“四点”
							进士（四进）：同时出现四个相同点数（除了四个四点外）
							举人（二举）：同时出现两个“四点”
							秀才（一秀）：博出一个“四点”
						</text>
					</view>
				</view>
			</uni-popup>
			<view class="iconfont icon-jiangbei" @click="openResultHistory()"></view>
			<uni-popup ref="HistoryPopup" type="center" @change="closeHistoryJudge()">
				<view class="result_history_wrapper">
					<view class="iconfont icon-fanhui" @click="$refs.HistoryPopup.close()"></view>
					<view class="history_title">
						<view>玩家</view>
						<view>状元</view>
						<view>榜眼</view>
						<view>探花</view>
						<view>进士</view>
						<view>举人</view>
						<view>秀才</view>
					</view>
					<view class="history_title" v-for="item in players" :key="item.key">
						<view>{{item.name}}</view>
						<view>{{item.player_award_history[1]}}</view>
						<view>{{item.player_award_history[2]}}</view>
						<view>{{item.player_award_history[3]}}</view>
						<view>{{item.player_award_history[4]}}</view>
						<view>{{item.player_award_history[5]}}</view>
						<view>{{item.player_award_history[6]}}</view>
					</view>
				</view>
			</uni-popup>
		</view>
		<view class="player_area">
			<view class="cloud">
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/d0f27ab6-cbac-410d-873a-f6eab039973f.png" mode="aspectFit"></image>
			</view>
			<view class="player_head">
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7ff8f646-346d-4825-843a-23e5b4930396.png" mode=""></image>
			</view>
		</view>
		<view class="star_area">
			<view class="star_shade" v-show="stopClickStar"></view>
			<view class="star" @click="playDices()" :animation="dragStarData">
				<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/2f90fb45-c8a0-4d83-a4ff-d29837122563.png" mode=""></image>
			</view>
		</view>
		<view class="judge_text" :animation="showResultAniData">
			{{result_dices[result_dices.length-1].award}}
		</view>
		<view class="dice_area">
			<view class="dice-wrap" v-for="item in show_dices" :key="item.key">
				<image :src="item.dice_url"></image>
			</view>
		</view>
		<view class="bottom">
			<scroll-view class="result_list" scroll-y="true" >
				<view class="result_list_item" v-for="(item,key) in result_dice_reverse" :key="item.key">
					<view class="result_item_index">
						{{result_dice_reverse.length-key}}
					</view> 
					<view class="result_player">
						{{item.player}}
					</view>
					<view class="result_item_text">
						{{item.award}}
					</view>
					<view class="result_item_image" v-for="dice in item.num" :key="dice.key">
						<image :src="dices[dice-1].dice_url"></image>
					</view>
				</view>
			</scroll-view>
			<image class="moon_rabbit" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/12873b5c-74cb-418b-8de1-3f00b660d71e.png" mode="widthFix"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				round_now:0,
				round:0,
				player_num_now:0,
				player_num:0,
				players:[],
				stopClickStar:false,
				dice_grade:-1,
				dragStarData:{},
				showResultAniData:{},
				result_dices:[],
				result_dice_counter:[],
				award_history:[
					{
						award:"状元",
						counter:0
					},
					{
						award:"榜眼",
						counter:0
					},
					{
						award:"探花",
						counter:0
					},
					{
						award:"进士",
						counter:0
					},
					{
						award:"举人",
						counter:0
					},
					{
						award:"秀才",
						counter:0
					}
				],
				dices:[
					{
						dice_num:1,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/34956d65-846c-462a-aead-647b1f5b6f73.png"
					},
					{
						dice_num:2,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/a20befa4-4a14-4563-afc4-cea000d78a97.png"
					},
					{
						dice_num:3,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7b61676f-9b74-4ca6-bec4-749b722c228b.png"
					},
					{
						dice_num:4,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/14aa38b2-686b-4092-a23a-d05a5fac64d3.png"
					},
					{
						dice_num:5,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/ec6d2cfa-7e66-4ae6-aabf-4eee3994a5d3.png"
					},
					{
						dice_num:6,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/2b25157c-1370-4a91-bf43-fc89bcf0d52a.png"
					},
					{
						dice_num:7,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/8db4bac4-a318-41cc-9fc0-5d09d9e520c6.gif"
					}
				],
				show_dices:[
					{
						dice_num:1,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/34956d65-846c-462a-aead-647b1f5b6f73.png"
					},
					{
						dice_num:2,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/a20befa4-4a14-4563-afc4-cea000d78a97.png"
					},
					{
						dice_num:3,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/7b61676f-9b74-4ca6-bec4-749b722c228b.png"
					},
					{
						dice_num:4,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/14aa38b2-686b-4092-a23a-d05a5fac64d3.png"
					},
					{
						dice_num:5,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/ec6d2cfa-7e66-4ae6-aabf-4eee3994a5d3.png"
					},
					{
						dice_num:6,
						dice_url:"https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/2b25157c-1370-4a91-bf43-fc89bcf0d52a.png"
					}
				],
			}
		},
		methods: {
			playDices(){
				this.stopClickStar = true
				this.$options.methods.dragStar.bind(this)()
				var num;
				let i;
				for (i=0;i<6;i++){
					this.show_dices[i].dice_url = this.dices[6].dice_url;
				}
				setTimeout(()=>{
					this.$options.methods.shuffleDice.bind(this)()
					this.$options.methods.judgeDice.bind(this)()
					this.$options.methods.showResultAni.bind(this)()
				},1000)
				setTimeout(()=>{
					this.stopClickStar = false
				},2000)
			},
			shuffleDice(){
				var num;
				for (let i=0;i<6;i++){
					num = Math.floor(Math.random()*6);
					this.show_dices[i].dice_num = this.dices[num].dice_num;
					this.show_dices[i].dice_url = this.dices[num].dice_url;
				}
			},
			dragStar(){
				this.animation = uni.createAnimation({duration: 700});
				this.animation.translateY(80).step({duration:200})
				this.dragStarData = this.animation.export();
				//回到初始位置
				setTimeout(()=>{
					this.animation.translateY(0).step({duration:350})
					this.dragStarData = this.animation.export();
				},400)
			},
			showResultAni(){
				this.animation = uni.createAnimation({
					duration:1000
				})
				this.animation.opacity(1).step({duration:300})
				this.showResultAniData = this.animation.export();
				setTimeout(()=>{
					this.animation.opacity(0).step({duration:350})
					this.showResultAniData = this.animation.export();
				},1000)
			},
			judgeDice(){
				let grade = [0,0,0,0,0,0]
				let i = 0
				let result = [0,0,0,0,0,0,0]
				for (i=0;i<6;i++){
					result[this.show_dices[i].dice_num]++;
				}
				// 状元判断
				for(i=1;i<=6;i++){
					if(result[i]===6||result[i]===5||(result[i]===4&&i==4)){
						grade[5] = 1
						break;
					}
				}
				//榜眼判断
				for(i=1;i<=6&&result[i]===1;i++);
				if (i===7){
					grade[4] = 1;
				}
				//探花,举人,秀才判断
				if (result[4] === 3){
					grade[3] = 1;
				}
				else if (result[4] === 2){
					grade[1] = 1;
				}
				else if (result[4] === 1){
					grade[0] = 1;
				}
				//进士判断
				for(i=1;i<=6;i++){
					if (result[i]===4&&i!==4){
						grade[2]=1;
						break;
					}
				}
				//结果输出
				let flag = 1;
				for(i=5;i>=0;i--){
					if (grade[i]===1){
						this.award_history[5-i].counter++
						this.dice_grade = i+1
						flag = 0;
					}
				}
				if (flag) this.dice_grade = -1
				this.$options.methods.showResult.bind(this)()
			},
			showResult(){
				let l = this.dice_grade
				let result = {
					player:"",
					award:"",
					num:[]
				}
				result.player = this.players[this.player_num_now].name
				for(let i = 0;i<6;i++){
					result.num.push(this.show_dices[i].dice_num)
				}
				if (l === 6){
					result.award = "状元"
				}
				else if (l === 5){
					result.award = "榜眼"
				}
				else if (l === 4){
					result.award = "探花"
				}
				else if (l === 3){
					result.award = "进士"
				}
				else if (l === 2){
					result.award = "举人"
				}
				else if (l === 1){
					result.award = "秀才"
				}
				else {
					result.award = "未中奖"
				}
				this.result_dices.push(result)
				this.players[this.player_num_now].player_award_history[l]++
				this.player_num_now++
				if (this.player_num_now >= this.player_num) this.round_now++
				this.player_num_now = this.player_num_now % this.player_num
				if (this.round <= this.round_now){
					this.$options.methods.EndGamePopup.bind(this)()
				}
			},
			quitGame(){
				uni.removeStorage({
				    key: 'player_info',
				    success: function (res) {
				    }
				});
				uni.removeStorage({
				    key: 'player_num',
				    success: function (res) {
				    }
				});
				uni.removeStorage({
				    key: 'round',
				    success: function (res) {
				    }
				});
				uni.redirectTo({
					url:'../index/index'
				})
			},
			openRule(){
				this.$refs.RulePopup.open('top')
			},
			openResultHistory(){
				this.$refs.HistoryPopup.open('top')
			},
			EndGamePopup(){
				this.$refs.EndPopup.open('top')
			},
			final_result(){
				this.$refs.EndPopup.close()
				this.$refs.HistoryPopup.open('top')
			},
			closeHistoryJudge(){
				if (this.round<=this.round_now){
					this.$refs.EndPopup.open('top')
				}
			},
			restartGame(){
				this.round_now = 0
				this.player_num_now = 0
				this.result_dices = []
				this.$refs.EndPopup.close()
			}
		},
		computed:{
			result_dice_reverse(){
				let [...arr] = this.result_dices
				return arr.reverse()
				
			}
		},
		onShow() {
			var that = this
			uni.getStorage({
			    key: 'player_info',
			    success: function (res) {
					that.players = res.data
			    }
			})
			uni.getStorage({
				key: 'player_num',
				success: function (res) {
					that.player_num = res.data
				}
			})
			uni.getStorage({
				key: 'round',
				success: function (res) {
					that.round = res.data
				}
			})
		}
	}
</script>

<style lang="scss">
	.bg{
		width: 100%;
		height: 100vh;
		margin: 0;
		padding: 0;
		position: relative;
		background: $bg-color;
		.EndPopup_wrapper{
			.EndPopup_table{
				display: flex;
				flex-direction: column;
				justify-content: center;
				width: 100%;
				height: 100vh;
				text-align: center;
				font-size: 70rpx;
				color:white;
				view:nth-child(1){
					display: flex;
					justify-content: center;
					align-items: center;
					height: 40%;
					font-size: 100rpx;
					font-weight: 700;
				}
				view:nth-child(n+1){
					display: flex;
					justify-content: center;
					align-items: center;
					
					height: 15%;
				}
			}
		}
		.top{
			position: absolute;
			width: 100%;
			height: 70rpx;
			z-index: 5;
			color: $icon-color;
			.icon-quit{
				position: relative;
				float: left;
				padding: 20rpx;
				font-size: 70rpx;
			}
			.room_info_wrapper{
				position: absolute;
				left:50%;
				transform: translateX(-50%);
				width: 200rpx;
				height: 80rpx;
				background-color: $bg-color;
				.room_info{
					position: absolute;
					top:16rpx;
					left:50%;
					padding-top: 7rpx;
					transform: translateX(-50%);
					width: 270rpx;
					height: 60rpx;
					text-align: center;
					font-size: 40rpx;
					border:6rpx solid $icon-color;
					border-radius: 20rpx;
					background-color: $bg-color;
				}
			}
			.icon-rule{
				position: relative;
				float: right;
				font-size: 70rpx;
				padding: 20rpx;
			}
			.icon-jiangbei{
				position: relative;
				float: right;
				font-size: 70rpx;
				padding: 20rpx;
			}
			.rule_text_wrapper{
				padding: 40rpx;
				view:nth-child(1){
					color:white;
					font-size: 70rpx;
				}
				.rule_title{
					margin-top: 100rpx;
					color:white;
					font-size: 110rpx;
					margin-bottom: 100rpx;
					text-align: center;
				}
				.rule_text{
					color: white;
					text{
						font-size: 39rpx;
					}
				}
			}
			.result_history_wrapper{
				padding: 40rpx;
				.icon-fanhui{
					color:white;
					font-size: 70rpx;
				}
				.history_title{
					display: flex;
					justify-content: center;
					width: 100%;
					margin-top: 100rpx;
					color:white;
					font-size: 40rpx;
					font-weight: 700;
					margin-bottom: 100rpx;
					text-align: center;
					view{
						text-align: center;
						width: 15%;
					}
				}
				.history_text{
					text-align: center;
					font-size: 70rpx;
					color: white;
					text{
						font-size: 60rpx;
					}
				}
			}
		}
		.star{
			position: absolute;
			left: 313rpx;
			top: -200rpx;
			z-index: 3;
			image{
				width: 128rpx;
				height: 678rpx;
			}
		}
		.star_shade{
			position: absolute;
			width: 128rpx;
			height: 678rpx;
			left: 313rpx;
			top: -200rpx;
			z-index: 4;
		}
		.dice_area{
			position: absolute;
			display: flex;
			flex-wrap: wrap;
			flex-direction: row;
			width: 500rpx;
			height: 400rpx;
			left: 125rpx;
			bottom: 300rpx;
			z-index: 2;
			.dice-wrap{
				width: 33%;
				display: flex;
				justify-content: center;
				align-items: center;
				image{
					width: 100rpx;
					height: 100rpx;
				}
			}

		}
		.judge_text{
			width: 100%;
			position: absolute;
			text-align: center;
			top:30vh;
			font-size: 150rpx;
			font-weight: 700;
			color: white;
			opacity: 0;
			z-index: 2;
		}
		.player_area{
			position: absolute;
			left: 52rpx;
			top: 7rpx;
			width: 640rpx;
			height: 500rpx;
			.cloud{
				position: absolute;
			}
			.player_head{
				position: relative;
				width: 640rpx;
				height: 500rpx;
				image:nth-child(1){
					position: absolute;
					top:92rpx;
					left:88rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(2){
					position: absolute;
					top:92rpx;
					left:430rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(3){
					position: absolute;
					top:147rpx;
					left:213rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(4){
					position: absolute;
					top:147rpx;
					left:558rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(5){
					position: absolute;
					top:200rpx;
					left:30rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(6){
					position: absolute;
					top:200rpx;
					left:372rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(7){
					position: absolute;
					top:275rpx;
					left:159rpx;
					width: 70rpx;
					height: 70rpx;
				}
				image:nth-child(8){
					position: absolute;
					top:275rpx;
					left:501rpx;
					width: 70rpx;
					height: 70rpx;
				}
			}
		}
		.result_list{
			position: absolute;
			bottom: 30rpx;
			left:10rpx;
			width: 60%;
			height: 18%;
			padding: 10rpx 4rpx;
			background-color: black;
			opacity: 0.5;
			border-radius: 20rpx;
			z-index: 4;
		}
		.result_list_item{
			color:white;
			display: flex;
			height: 40rpx;
			justify-content: center;
			text-align: center;
			font-size: 25rpx;
			.result_item_index{
				width: 20rpx;
			}
			.result_player{
				width: 120rpx;
			}
			.result_item_text{
				width: 100rpx;
			}
			.result_item_image{
				image{
					padding-left: 4rpx;
					width: 30rpx;
					height: 30rpx;
				}
			}
		}
		.moon_rabbit{
			position: absolute;
			width: 100%;
			bottom: 0;
		}
	}
</style>
