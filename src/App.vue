<template>

<div id="app">
	<div class="bubble-wrapper">
		<div ref="bubble" class="bubble">
			<img ref="bubbleImage" class="bubble-image" :src="currentLogo" />
		</div>
		<div ref="bubblePulse" class="bubble-pulse"></div>
	</div>
	<button @click="randomiseLogo">Random Logo</button>
</div>

</template>

<script>

	import { TimelineLite, Back, Elastic, Expo } from "gsap"

	export default {
		name: 'app',
		data() {
			return {
				timeline: null,
				logos: [
					'assets/img/slack-white.svg',
					'assets/img/discord-white.svg',
					'assets/img/messenger-white.png',
				],
				currentLogo: ''
			}
		},
		methods: {
			randomiseLogo() {
				const logosToSample = this.logos.filter(logo => logo !== this.currentLogo)
				this.currentLogo = logosToSample[Math.floor(Math.random() * logosToSample.length)]
			},
			loopAnimation() {
				this.randomiseLogo()
				this.timeline.restart()
			},
		},
		mounted() {

			this.randomiseLogo()

			const { bubble, bubblePulse, bubbleImage } = this.$refs;

		 	this.timeline = new TimelineLite({
				onComplete: () => {
					this.loopAnimation()
				}
			});

			this.timeline.from(bubbleImage, 0.4, { opacity: 0 })
			this.timeline.to(bubble, 0.4, {scale: 0.8, rotation: 16, ease: Back.easeOut.config(1.7)});
			this.timeline.to(bubblePulse, 0.8, {scale: 0.9, opacity: 1},"-=0.6");
			this.timeline.to(bubble, 1.2, {scale: 1, rotation: "-=16", ease: Elastic.easeOut.config(2.5, 0.5)});
			this.timeline.to(bubblePulse, 1.1, {scale: 3, opacity: 0, ease: Expo.easeOut}, "-=1.2");
			this.timeline.to(bubbleImage, 0.2, { opacity: 0 })
		}
	}
</script>

<style>

.bubble-wrapper {
	position: relative;
}

.bubble {
	position: relative;
	z-index: 2;
	display: flex;
	align-items: center;
	justify-content: center;
	border: 1px solid white;
	background: #272727;
	border-radius: 50%;
	height: 100px;
	width: 100px;
	box-shadow: 0 2px 6px rgba(0, 0, 0, 0.4);
}

.bubble-pulse {
	position: absolute;
	z-index: 1;
	top: 50%;
	left: 50%;
	margin-top: -60px;
	margin-left: -60px;
	background: rgba(255, 255, 255, 0.1);
	height: 120px;
	width: 120px;
	border-radius: 50%;
	opacity: 0;
	transform: scale(0);
}

.bubble-image {
	width: 50%;
}

body {
	background: #1a1a1a;
	padding-top: 80px;
	display: flex;
	align-items: center;
	justify-content: center;
}

button {
	margin-top: 40px;
	position: relative;
	z-index: 1000;
}

</style>
