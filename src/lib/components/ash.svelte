<script lang="ts">
	import { onMount } from 'svelte';

	interface AshParticle {
		x: number;
		y: number;
		a: number;
		r: number;
		g: number;
		b: number;
		dp: { x: number; y: number }[];
	}

	interface AshOptions {
		x?: number;
		y?: number;
		a?: number;
		dp?: { x: number; y: number }[];
	}

	function randomRange(from: number, to: number, seed?: number): number {
		return Math.floor((seed ?? Math.random()) * (to - from + 1) + from);
	}

	onMount(() => {
		const cntr = document.getElementById('canvascontainer');
		if (!cntr) return;

		const W = cntr.offsetWidth;
		const H = cntr.offsetHeight;
		const canvas = [
			document.getElementById('canvas') as HTMLCanvasElement,
			document.getElementById('buffer') as HTMLCanvasElement
		];
		const ctxs = [canvas[0].getContext('2d')!, canvas[1].getContext('2d')!];
		let C = 0;
		let angle = 0;
		const A: AshParticle[] = [];

		function createAsh(o?: AshOptions): AshParticle {
			const m = Math.random();
			const p = randomRange(4, 8, m);

			const particle: AshParticle = {
				x: o?.x ?? m * W,
				y: o?.y ?? m * H,
				a: o?.a ?? m * (p - 4) + 1,
				r: randomRange(233, 255, m),
				g: randomRange(181, 192, m),
				b: randomRange(72, 88, m),
				dp: o?.dp ?? [{ x: 0, y: 0 }]
			};

			if (!o?.dp) {
				for (let i = 0; i < p; i++) {
					const j = i === 0 || p / 2 > i ? 1 : -1;
					particle.dp.push({
						x: particle.dp[i].x + randomRange(5, 30) * j,
						y: particle.dp[i].y + randomRange(5, 30) * j
					});
				}
			}

			return particle;
		}

		function draw() {
			if (C === 0) {
				canvas[0].style.visibility = 'visible';
				canvas[1].style.visibility = 'hidden';
				C = 1;
			} else {
				canvas[1].style.visibility = 'visible';
				canvas[0].style.visibility = 'hidden';
				C = 0;
			}

			const ctx = ctxs[C];
			ctx.clearRect(0, 0, W, H);

			for (let i = 0; i < A.length; i++) {
				const p = A[i];
				const grad = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, p.a);
				grad.addColorStop(0, `rgba(${p.r}, ${p.g}, ${p.b}, 1)`);
				grad.addColorStop(0.9, `rgba(${p.r}, ${p.g}, ${p.b}, ${randomRange(1, 10) / 10})`);
				grad.addColorStop(1, `rgba(${p.r}, ${p.g}, ${p.b}, 0)`);

				ctx.beginPath();
				ctx.moveTo(p.x, p.y);
				for (let j = 1; j < p.dp.length; j++) {
					ctx.lineTo(p.x + p.dp[j].x, p.y + p.dp[j].y);
				}
				ctx.closePath();
				ctx.fillStyle = grad;
				ctx.globalAlpha = 0.7;
				ctx.fill();
			}

			update();
		}

		function update() {
			angle += 0.01;

			for (let i = 0; i < A.length; i++) {
				const p = A[i];

				p.y += Math.cos(angle + A.length) + 1 + p.a / 2;
				p.x += Math.sin(angle) * 2;

				if (p.x > W + 5 || p.x < -5 || p.y > H) {
					if (i % 3 > 0) {
						A[i] = createAsh({ y: -10, a: p.a, dp: p.dp });
					} else {
						if (Math.sin(angle) > 0) {
							A[i] = createAsh({ x: -5, a: p.a, dp: p.dp });
						} else {
							A[i] = createAsh({ x: W + 5, a: p.a, dp: p.dp });
						}
					}
				}
			}
		}

		canvas[0].width = W;
		canvas[0].height = H;
		canvas[1].width = W;
		canvas[1].height = H;

		for (let i = 0; i < 50; i++) {
			A.push(createAsh());
		}

		const interval = setInterval(draw, 33);

		return () => clearInterval(interval);
	});
</script>

<div id="canvascontainer">
	<canvas id="buffer"></canvas>
	<canvas id="canvas"></canvas>
</div>

<style>
	#canvascontainer {
		width: 100%;
		height: 100%;
		position: absolute;
		inset: 0;
		pointer-events: none;
	}

	canvas {
		position: absolute;
		width: 100%;
		height: 100%;
		transform: translate3d(0, 0, 0);
	}
</style>
