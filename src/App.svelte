<script lang="ts">
	import Game from "./Game.svelte";

	let attempts = 0;
	let hits = 0;

	function hit() {
		attempts++;
		hits++;
	}
	function miss() {
		attempts++;
	}
</script>

<main>
	<h1>Celeste 2 Trainer</h1>
	<Game hit={hit} miss={miss}/>
	{#if attempts === 0}
		<p>You have done no attempts.</p>
	{:else}
		<p>You've done {hits}/{attempts} ({(hits/attempts * 100).toFixed(2)}%) successful attempts</p>
	{/if}
	<button on:click={() => {attempts = 0; hits = 0;}}>Reset</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}

	h1 {
		color: #ffffff;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (prefers-color-scheme: light) {
		h1 {
			color: #ff3e00;
		}
	}
</style>