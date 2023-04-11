<script lang="ts">
	import { page } from '$app/stores';
	import { fade, scale, blur } from 'svelte/transition';
	import Header from './Header.svelte';
	import './styles.css';
	import { cubicIn } from 'svelte/easing';

	export let data;

	function scaleLR(
		node: Element,
		{ delay = 0, duration = 250, easing = cubicIn } = {},
		{ direction = 'both' } = {}
	) {
		const origin = {
			in: 'bottom left',
			out: 'bottom right',
			both: 'center center'
		};

		return {
			delay,
			duration,
			easing,
			css: (t: number, u: number) => {
				return `
					scale: ${t};
					transform-origin: ${origin[direction]};
				`;
			}
		};
	}

	function flush(
		node: Element,
		{ delay = 0, duration = 250, easing = cubicIn } = {},
		{ direction = 'both' } = {}
	) {
		return {
			delay,
			duration,
			easing,
			css: (t: number, u: number) => {
				return `
					scale: ${t};
					rotate: ${t}turn;
				`;
			}
		};
	}
</script>

<div class="app">
	<Header />

	<main>
		{#key data.url}
			<div in:flush={{ duration: 250, delay: 250 }} out:flush={{ duration: 250 }}>
				<slot />
			</div>
		{/key}
	</main>

	<footer>
		<p>visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to learn SvelteKit</p>
	</footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
