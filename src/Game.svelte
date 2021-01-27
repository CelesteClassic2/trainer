<script lang="ts">
	import { onMount } from "svelte";

	export let hit;
	export let miss;

	const FRAMERATE = 30;

	let canvas: HTMLCanvasElement;
	let context: CanvasRenderingContext2D;

	type Object = {
		x: number,
		y: number,
		sprite: HTMLImageElement,
	}

	type Player = Object & {
		speed_x: number,
		speed_y: number,
		grapple_timer: number,
	};
	
	let player: Player = {x: 48, y: 64, speed_x: 0, speed_y: 0, sprite: null, grapple_timer: 0};

	let block: Object = {x: 80, y: 64, sprite: null};

	function update() {
		if (player.grapple_timer === 0) {
			context.clearRect(0, 0, canvas.width, canvas.height);

			const x = Math.floor(player.x);
			const y = Math.floor(player.y);
			
			context.drawImage(player.sprite, x, y);
			if (y < 64) {
				context.drawImage(player.sprite, x, y + 128);
			} else {
				context.drawImage(player.sprite, x, y - 128);
			}
			
			context.drawImage(block.sprite, block.x, block.y);
	
			// Gravity: self.speed_y=min(self.speed_y+(abs(self.speed_y)<0.2 and 0.4 or 0.8),btn(3) and 5.2 or 4.5)
			player.speed_y = Math.min(player.speed_y + (Math.abs(player.speed_y) < 0.2 ? 0.4 : 0.8), input.down ? 5.2 : 4.5);
	
			if (input.grapple) {
				input.grapple = false;
				player.grapple_timer = 10;
				
				
				const inRange = player.y >= 59 && player.y <= 61;
				if (inRange) hit(); else miss();				

				context.beginPath();
				context.filter = "url(#remove-alpha)";
				context.strokeStyle = inRange ? "green" : "red";
				context.moveTo(x + 5.5, y + 5.5);
				context.lineTo(x + 128.5, y + 5.5);
				context.stroke();

				context.beginPath();
				context.filter = "url(#remove-alpha)";
				context.strokeStyle = inRange ? "darkgreen" : "darkred";
				context.moveTo(x + 5.5, y + 6.5);
				context.lineTo(x + 128.5, y + 6.5);
				context.stroke();
			}

			player.x += player.speed_x;
			player.y += player.speed_y;
	
			player.y %= 128;
		} else {
			player.grapple_timer--;
		}

		setTimeout(update, 1000 / FRAMERATE);
	}

	const input = {
		grapple: false,
		down: false,
	};

	onMount(() => {
		context = canvas.getContext("2d");
		update();

		document.addEventListener("keydown", event => {
			if (event.repeat) { return }
			if (event.code === "ArrowDown") {
				input.down = true;
			}
			if (event.code === "KeyX") {	
				input.grapple = true;
			}
		});
		document.addEventListener("keyup", event => {
			if (event.code === "ArrowDown") {
				input.down = false;
			}
			if (event.code === "KeyX") {
				input.grapple = false;
			}
		});
	});


</script>

<svg width="0" height="0" style="position:absolute;z-index:-1;">
	<defs>
	  <filter id="remove-alpha" x="0" y="0" width="100%" height="100%">
		<feComponentTransfer>
		  <feFuncA type="discrete" tableValues="0 1"></feFuncA>
		</feComponentTransfer>
		</filter>
	</defs>
</svg>
<canvas bind:this={canvas} width="128" height="128"></canvas>
<div>
	<img bind:this={player.sprite} src="./assets/player.png" alt="">
	<img bind:this={block.sprite} src="./assets/block.png" alt="">
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