<script lang="ts">
	import { onMount } from "svelte";

	const FRAMERATE = 30;

	let canvas: HTMLCanvasElement;
	let context: CanvasRenderingContext2D;

	type Object = {
		x: number,
		y: number,
		speed_x: number,
		speed_y: number,
		sprite: HTMLImageElement,
	}

	let player: Object = {x: 64, y: 64, speed_x: 0, speed_y: 0, sprite: null};

	function update() {
		context.clearRect(0, 0, canvas.width, canvas.height);

		context.drawImage(player.sprite, Math.floor(player.x), Math.floor(player.y));

		player.x += player.speed_x;
		player.y += player.speed_y;
		
		player.speed_x += Math.random() - 0.5;
		player.speed_y += Math.random() - 0.5;

		setTimeout(update, 1000 / FRAMERATE);
	}

	onMount(() => {
		context = canvas.getContext("2d");
		update();
	});


</script>

<canvas bind:this={canvas} width="128" height="128"></canvas>
<div>
	<img bind:this={player.sprite} src="./assets/player.png" alt="">
</div>

<style>
	canvas {
		width: 512px;
		height: 512px;
		background-color: black;
		/* https://stackoverflow.com/a/7665647 */
		image-rendering: optimizeSpeed;
		image-rendering: -moz-crisp-edges;
		image-rendering: -webkit-optimize-contrast;
		image-rendering: optimize-contrast;
		image-rendering: pixelated;
		-ms-interpolation-mode: nearest-neighbor;
	}

	div {
		display: none;
	}
</style>