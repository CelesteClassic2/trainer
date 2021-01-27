<script lang="ts">
	import Game from "./Game.svelte";

	let attempts = 0;
	let hits = 0;
</script>

<main>
	<h1>Celeste 2 Trainer</h1>
	<Game />
	<!-- <button on:click={() => {attempts++; hits++;}}>Hit</button>
	<button on:click={() => {attempts++;}}>Miss</button> -->
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
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
</style>