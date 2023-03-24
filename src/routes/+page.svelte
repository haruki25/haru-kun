<script>
	import HeroTitle from '../components/sakura_petals/HeroTitle.svelte';
	import SocialsHost from '../components/haruki/SocialsHost.svelte';
	import Hero from '../components/haruki/Hero.svelte';
	import RichPresence from '../components/haruki/RichPresence.svelte';
	import { onMount } from 'svelte';
	import ColorThief from '/node_modules/colorthief/dist/color-thief.mjs';

	const colorThief = new ColorThief();
	onMount(async () => {
		const avatar = document.querySelector('.logo');
		const avatarUrl = avatar.getAttribute('src');

		async function getAverageColor(avatar) {
			const color = await colorThief
				.getColor(avatar)
				.map((x) => {
					const hex = x.toString(16);
					return hex.length === 1 ? '0' + hex : hex;
				})
				.join('');
			console.log(color);
			return color;
		}

		function setColors() {
			document.documentElement.style.setProperty(
				'--background-color',
				localStorage.getItem('backgroundColor')
			);
			document.documentElement.style.setProperty('--primary', localStorage.getItem('pinky'));
			document.documentElement.style.setProperty('--secondary', localStorage.getItem('darkPinky'));
		}

		avatar.onload = async () => {
			const averageColor = await getAverageColor(avatar);
			console.log(avatarUrl);

				localStorage.setItem('avatarUrl', avatarUrl);

				async function getColors() {
					const json = await fetch(
						`https://cors-anywhere.azm.workers.dev/https://www.tints.dev/api/brand/${averageColor}`
					).then((r) => r.json());
					console.log(json);
					return json;
				}
				const colors = await getColors();
				localStorage.setItem('backgroundColor', colors.brand['900']);
				localStorage.setItem('pinky', colors.brand['500']);
				localStorage.setItem('darkPinky', colors.brand['800']);
				setColors();
			}
	});
</script>

<div class="center">
	<div class="wrapper">
		<Hero />
		<div class="introduction-container">
			<HeroTitle />
			<span class="introduction">
				Konnichiwa! Just a guy whose love for anime and coding collide to create something as unique as a unicorn in a sea of horses.
			</span>
			<div class="social-buttons">
				<SocialsHost />
			</div>
			<RichPresence />
		</div>
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		min-height: -webkit-fill-available;
		min-height: -moz-available;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: var(--background-color);
	}

	:global(html) {
		height: -webkit-fill-available;
		height: -moz-available;
	}

	:root {
		--background-color: #130f14;
		--white: #d1d0d0;
		--primary: #5d4b64;
		--secondary: #261f29;
		--font-one: 'Space Grotesk', sans-serif;
		--font-two: 'JetBrains Mono', monospace;
	}

	.wrapper {
		display: flex;
		max-width: 100vw;
		height: 100%;
	}

	.center {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.introduction-container {
		display: flex;
		justify-content: center;
		flex-direction: column;
		gap: 2rem;
		color: var(--primary);
		font-family: var(--font-one);
	}

	.introduction-container .introduction {
		color: var(--white);
		font-size: 1.3rem;
		font-weight: 300;
	}

	@media (max-width: 868px) {
		.wrapper {
			flex-direction: column;
			gap: 2rem;
			width: min(90%, 65rem) !important;
		}

		.introduction-container {
			align-items: center;
			font-family: var(--font-one);
			font-size: 6px;
		}

		.introduction-container .introduction {
			text-align: center;
			font-size: 16px;
			padding-right: 0;
			width: 90%;
		}

		.social-buttons {
			width: 100%;
			display: flex;
			justify-content: center;
			gap: 1rem;
			align-items: center;
		}
	}
</style>