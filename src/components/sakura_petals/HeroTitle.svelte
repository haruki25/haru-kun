<script>
	import { onMount } from 'svelte';
	import LanyardData from '../../stores/LanyardStore';

	let username = 'haruki';

	onMount(() => {
		LanyardData.subscribe(async (e) => {
			let data = await e;
			username = data.data.discord_user.username;
		});
	});
</script>

<span>hey, i'm <strong>{username.toLowerCase()}</strong></span>

<style>
	span {
		font-size: 4rem;
		font-family: var(--font-one);
		color: var(--primary);
		margin-bottom: -1.5rem;
	}

	@media (max-width: 868px) {
		span {
			display: none;
		}
	}
</style>
