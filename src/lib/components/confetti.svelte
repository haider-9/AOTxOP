<script lang="ts">
	import { onMount } from 'svelte';

	interface ConfettiParticle {
		x: number;
		y: number;
		rotation: number;
		rotationSpeed: number;
		speed: number;
		size: number;
		color: string;
		sway: number;
		swaySpeed: number;
	}

	const colors = ['#fbbf24', '#f59e0b', '#dc2626', '#ef4444', '#fb923c', '#fdba74'];

	onMount(() => {
		const canvas = document.getElementById('confetti-canvas') as HTMLCanvasElement;
		if (!canvas) return;

		const ctx = canvas.getContext('2d')!;
		const W = canvas.width = canvas.offsetWidth;
		const H = canvas.height = canvas.offsetHeight;
		const particles: ConfettiParticle[] = [];

		function createConfetti(): ConfettiParticle {
			return {
				x: Math.random() * W,
				y: -20,
				rotation: Math.random() * 360,
				rotationSpeed: (Math.random() - 0.5) * 4,
				speed: Math.random() * 2 + 1,
				size: Math.random() * 6 + 3,
				color: colors[Math.floor(Math.random() * colors.length)],
				sway: Math.random() * 2 - 1,
				swaySpeed: Math.random() * 0.02 + 0.01
			};
		}

		function drawConfetti(p: ConfettiParticle) {
			ctx.save();
			ctx.translate(p.x, p.y);
			ctx.rotate((p.rotation * Math.PI) / 180);
			ctx.fillStyle = p.color;
			ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size);
			ctx.restore();
		}

		function updateConfetti(p: ConfettiParticle) {
			p.y += p.speed;
			p.rotation += p.rotationSpeed;
			p.x += Math.sin(p.y * p.swaySpeed) * p.sway;

			if (p.y > H + 20) {
				const index = particles.indexOf(p);
				particles.splice(index, 1);
				particles.push(createConfetti());
			}
		}

		function animate() {
			ctx.clearRect(0, 0, W, H);

			particles.forEach((p) => {
				drawConfetti(p);
				updateConfetti(p);
			});

			requestAnimationFrame(animate);
		}

		// Initialize particles
		for (let i = 0; i < 30; i++) {
			const p = createConfetti();
			p.y = Math.random() * H;
			particles.push(p);
		}

		animate();
	});
</script>

<canvas id="confetti-canvas" class="absolute inset-0 pointer-events-none opacity-40"></canvas>

<style>
	canvas {
		width: 100%;
		height: 100%;
	}
</style>
