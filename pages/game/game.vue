<template>
	<view class="bg">
		<view class="cloud">
			<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/d0f27ab6-cbac-410d-873a-f6eab039973f.png" mode="aspectFit"></image>
		</view>
		<view class="star" @click="playDices()" :animation="dragStarData">
			<image src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/2f90fb45-c8a0-4d83-a4ff-d29837122563.png" mode=""></image>
		</view>
		<view class="judge_text" :animation="showResultAniData">
			{{showResult()}}
		</view>
		<view class="dice_area">
			<view class="dice-wrap" v-for="item in show_dices" :key="item.key">
				<image :src="item.dice_url"></image>
			</view>
		</view>
		<view class="bottom">
			<image class="moon_rabbit" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-112433b9-5f86-40f2-9487-4c51511869dc/12873b5c-74cb-418b-8de1-3f00b660d71e.png" mode="widthFix"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dice_grade:-1,
				dragStarData:{},
				showResultAniData:{},
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
				this.$options.methods.dragStar.bind(this)()
				var num;
				let i;
				setTimeout(()=>{
					this.$options.methods.shuffleDice.bind(this)()
					this.dice_grade = this.$options.methods.judgeDice.bind(this)()
					this.$options.methods.showResultAni.bind(this)()
				},1000)
				for (i=0;i<6;i++){
					this.show_dices[i].dice_url = this.dices[6].dice_url;
				}
			},
			shuffleDice(){
				var num;
				for (let i=0;i<6;i++){
					num = Math.floor(Math.random()*6);
					this.show_dices[i].dice_num = this.dices[num].dice_num;
					this.show_dices[i].dice_url = this.dices[num].dice_url;
					// console.log(this.show_dices[i])
				}
			},
			dragStar(){
				this.animation = uni.createAnimation({duration: 1000});
				this.animation.translateY(80).step({duration:200})
				this.dragStarData = this.animation.export();
				//回到初始位置
				setTimeout(()=>{
					this.animation.translateY(0).step({duration:350})
					this.dragStarData = this.animation.export();
				},400)
				console.log("已经调用了动画")
			},
			showResultAni(){
				this.animation = uni.createAnimation({
					duration:1000
				})
				this.animation.opacity(1).step({duration:300})
				this.showResultAniData = this.animation.export();
				setTimeout(()=>{
					console.log("准备回到初始")
					this.animation.opacity(0).step({duration:350})
					this.showResultAniData = this.animation.export();
				},1000)
				console.log("调用了文字动画")
			},
			judgeDice(){
				let grade = [0,0,0,0,0,0]
				let i = 0
				let result = [0,0,0,0,0,0,0]
				for (i=0;i<6;i++){
					result[this.show_dices[i].dice_num]++;
					console.log(this.show_dices[i].dice_num)
				}
				console.log(result)
				
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
				console.log(result)
				console.log("grade"+grade)
				console.log("完成判定")
				//结果输出
				for(i=5;i>=0;i--){
					if (grade[i]===1){
						return i+1
					}
				}
				return -1
			},
			showResult(){
				var l = this.dice_grade
				if (l === 6){
					return "状元"
				}
				else if (l === 5){
					return "榜眼"
				}
				else if (l === 4){
					return "探花"
				}
				else if (l === 3){
					return "进士"
				}
				else if (l === 2){
					return "举人"
				}
				else if (l === 1){
					return "秀才"
				}
				else {
					return "未中奖"
				}
			}
		},
		onLoad() {
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
		background: #264a51;
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
		.cloud{
			position: absolute;
			left: 52rpx;
			top: -67rpx;
		}
		.moon_rabbit{
			position: absolute;
			width: 100%;
			bottom: 0;
		}
	}
</style>
