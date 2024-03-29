<script lang="ts">
import { onMount } from "svelte";
import { clickables, homeAnchor, workAnchor, aboutAnchor, loadPagePromise } from "../store";
import anime from "animejs";

let home, work, about, email, logo, github, mobileMenu;
let homeLink, workLink, aboutLink;
let mobileActive;
export let scrollContainer;

onMount(async () => {
	await loadPagePromise;

	clickables.update(value => [...value, home, work, about, email, logo, github, mobileMenu]);

	let targets = [logo, mobileMenu, homeLink, workLink, aboutLink, github];

	targets.forEach(e => {
		e.style.transform = "translateY(125%) rotate(10deg)"
	})

	// Intro animation on page load
	anime({
		targets: targets,
		rotate: 0,
		translateY: "0%",
		easing: "cubicBezier(0.165, 0.84, 0.44, 1)",
		duration: 1500,
		delay: anime.stagger(50, {start: + 3000})
	});
});


// Scroll positions fetched from svelte store that are updated by each component itself
let anchors = { home: 0, work: 0, about: 0 }
homeAnchor.subscribe(val => anchors.home = val);
workAnchor.subscribe(val => anchors.work = val);
aboutAnchor.subscribe(val => anchors.about = val);

const navigate = anchor => { 
	scrollContainer.scrollTo({
		top: anchor.offsetTop - (window.innerHeight / 5),
		behavior: "smooth"
	});
	mobileActive = false; 
}

</script>



<div class="nav-wrapper" style="transform: translate(0px);">
	<div class="flex-wrapper ico" style="z-index: 21;">
		<img 
			bind:this={logo} 
			src="assets/imgs/logo.svg"
			class = "logo-icon clickable"
			alt="Logo"
			draggable="false"
			on:click={() => navigate(anchors.home)}>
	</div>
	
	<div class="flex-wrapper">
		<div class="wrapper" class:mobileActive>
			<ul class="nav-list">
				<li bind:this={homeLink}>
					<div bind:this={home} on:click={() => navigate(anchors.home)}>Home</div>
				</li>
				<li bind:this={workLink}>
					<div bind:this={work} on:click={() => navigate(anchors.work)}>Work</div>
				</li>
				<li bind:this={aboutLink}>
					<div bind:this={about} on:click={() => navigate(anchors.about)}>About</div>
				</li>
				<li class="mobile" bind:this={email}>
					<a href="mailto:vk.ind004@gmail.com" target="_blank">Email</a>
				</li>
				<li bind:this={github}>
					<a href="https://github.com/sylvester-dev" target="_blank">Github</a>
				</li>
			
		</div>

		<div class="mask">
			<div class="hb-button clickable" 
				bind:this={mobileMenu} 
				on:click={() => mobileActive = !mobileActive} 
				class:mobileActive>

				<div class="hb">
					<span></span>
					<span></span>
					<span></span>
				</div>
				<div class="text">Menu</div>
			</div>
		</div>
	</div>
</div>



<style lang="sass">

@import "../consts.sass"

.nav-wrapper
	width: 100vw
	margin: 0 auto
	padding: 0 7vw
	box-sizing: border-box
	display: block
	z-index: 100
	display: flex
	flex-direction: row
	justify-content: space-between
	align-items: center

	.flex-wrapper.ico
		overflow: hidden
		height: 6vh
		width: 7vh
		mix-blend-mode: exclusion
		cursor: pointer

		.logo-icon
			position: relative
			display: inline-block
			height: 100%
			width: 100%

	@media only screen and (min-width: 950px)
		ul.nav-list
			list-style-type: none
			mix-blend-mode: exclusion
			overflow: hidden

			li
				font-family: $font
				text-transform: uppercase
				font-size: 2vh
				letter-spacing: 0.2vh
				display: inline-block

				&.mobile
					display: none

				div 
					display: inline-block
					cursor: pointer

				a
					color: white
					text-decoration: none

				&:not(.mobile):not(:last-child)::after
					margin-right: 0.3vw
					margin-left: 0.5vw
					content: "-"

	@media only screen and (max-width: 950px)
		.wrapper
			position: fixed
			top: -10vh
			left: 0
			height: 100vh
			width: 0vw
			background-color: #18181a
			transition: 1s cubic-bezier(0.86, 0, 0.07, 1) width
			-webkit-transition: 1s cubic-bezier(0.86, 0, 0.07, 1) width
			-moz-transition: 1s cubic-bezier(0.86, 0, 0.07, 1) width
			overflow: hidden !important

			ul.nav-list
				list-style-type: none
				display: flex
				flex-direction: column
				position: relative
				justify-content: center
				height: 100%
				width: 100%
				box-sizing: border-box
				padding: 0 10vw
				padding-top: 10vh
				overflow: hidden !important

			&.mobileActive
				width: 100vw

			li
				font-family: $font
				font-weight: bolder
				text-transform: lowercase
				font-size: 9vw
				display: inline-block
				cursor: pointer
				padding: 2vh 0
				box-sizing: border-box
				border-bottom: 1px solid white

				div 
					display: inline-block

				a
					color: white
					text-decoration: none

	.mask
		overflow: hidden

	.hb-button
		cursor: pointer
		position: relative
		z-index: 21

		*
			display: inline-block
			vertical-align: middle
			user-select: none
			-ms-user-select: none
			-moz-user-select: none

		.text
			font-family: $font
			font-size: 2.5vh
			text-transform: lowercase

		.hb
			width: 3vh
			height: 2.1vh
			margin-right: 1.5vh

			span
				transition: 1s ease
				-webkit-transition: 1s ease
				-moz-transition: 1s ease
				display: block
				position: relative
				top: 0
				right: 0
				height: 2px
				width: 100%
				background-color: white

				&:nth-child(1)
					top: 0

				&:nth-child(2)
					top: 0.5vh

				&:nth-child(3)
					top: 1.1vh

		&.mobileActive
			.text
				color: white

			.hb
				span
					background-color: white
					top: 0.5vh

					&:nth-child(1)
						transform: translateY(100%) rotate(-45deg)
						width: 100%

					&:nth-child(2)
						width: 0

					&:nth-child(3)
						transform: translateY(-100%) rotate(45deg)
						width: 100%

@media only screen and (min-width: 950px)
	.hb-button
		display: none

@media only screen and (max-width: 950px)
	.hb-button
		display: block

</style>