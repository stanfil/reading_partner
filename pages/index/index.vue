<template>
	<view class="main">
		<view class="timer">
			<p class="timer-se">
				<text>{{toTime(timer.start)}}</text>
				<span>~</span>
				<text>{{toTime(timer.end)}}</text>
			</p>
			
			<text>本次时长：{{Math.round(timer.duration / (3600 * 1000))}}小时</text>
			<text>总读书时长：{{Math.round(timer.total / (3600 * 1000))}}小时</text>
			<button :tap="tapTimer">{{timer.status === 0 ? "start" : "end"}}</button>	
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				book: {
					id: 0,
					title: '',
					startTime: 0,
					endTime: 0,
					duration: 0,
					type: 0, // 0: 小说
					score: 10,
					progress: 0.5,
					imgUrl: '',
					comment: '',
				},
				timer: {
					start: 0,
					end: 0,
					status: 0, // 0: to start,
					total: 0,
					duration: 0
				},
				plan: [
					{
						id: 0,
						title: 0,
						progress: 0
					}
				],
				cloud: {
					timer: {}
				}
			}
		},
		async onLoad() {
			const db = wx.cloud.database()
			db.collection('timer').doc('aaad2e2a-d024-47c5-9bf6-400f7286e422').get().then(res => {
				console.log(res)
			})
			
			// this.cloud.timer = timer
			// this.timer = await timer.get()
		},
		methods: {
			tapTimer () {
				this.timer.status = this.timer.status ? 0 : 1
				if (this.timer.status) {
					const start = new Date()
					this.timer.start = start
					this.timer.end = 0
					this.timer.timer.start = start
				} else {
					const end = new Date()
					this.timer.end = end
					// this.timer.timer.end = end
					this.timer.duration = this.timer.end - this.timer.start
					// this.timer.timer.duration = this.timer.duration
					this.timer.total += this.timer.duration
					this.timer.timer = {
						start: this.timer.start,
						end: this.timer.end,
						status: this.timer.status,
						total: this.timer.total,
						duration: this.timer.duration
					}
				}
			},
			
			toTime: function (data) {
				if (!data) {
					return 0
				}
				const tArr = [date.getHours(), date.getMinutes(), date.getSeconds()]
				return tArr.join(':')
			},
		},
	}
</script>

<style lang="scss">
.timer {
	display: flex;
	flex-direction: column;
	align-items: center;
	.timer-se {
		display: flex;
		justify-content: space-between;
		margin: 0 15vw;
	}
}
</style>
