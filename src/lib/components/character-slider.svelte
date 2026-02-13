<script lang="ts">
	import { fly, scale } from 'svelte/transition';
	import { cubicOut } from 'svelte/easing';
	import { Card } from './ui/card';

	type Character = {
		name: string;
		role: string;
		image: string;
	};

	type Props = {
		characters: Character[];
		theme?: 'onepiece' | 'aot';
	};

	let { characters, theme = 'onepiece' }: Props = $props();

	let currentIndex = $state(0);
	let direction = $state(1);

	const nextSlide = () => {
		direction = 1;
		currentIndex = (currentIndex + 1) % characters.length;
	};

	const prevSlide = () => {
		direction = -1;
		currentIndex = (currentIndex - 1 + characters.length) % characters.length;
	};

	const getCardIndex = (offset: number) => {
		return (currentIndex + offset + characters.length) % characters.length;
	};


</script>

<div class="relative w-full h-screen overflow-hidden">
	<div class="flex h-full">
		<!-- Left Side Card (Half Visible) -->
		<div class="relative w-1/4 overflow-hidden">
			{#key currentIndex}
				<div
					class="absolute inset-0"
					in:fly={{ x: direction > 0 ? -300 : 300, duration: 500, easing: cubicOut }}
					out:fly={{ x: direction > 0 ? 300 : -300, duration: 500, easing: cubicOut }}
				>
					<img
						src={characters[getCardIndex(-1)].image}
						alt={characters[getCardIndex(-1)].name}
						class="h-full w-full object-cover brightness-75"
					/>
					<div class="absolute inset-0 bg-black/40"></div>
					<div class="absolute bottom-8 left-4 right-4">
						<h3 class="text-2xl font-bold text-white drop-shadow-lg">
							{characters[getCardIndex(-1)].name}
						</h3>
						<p class="text-sm text-white/80 drop-shadow">
							{characters[getCardIndex(-1)].role}
						</p>
					</div>
				</div>
			{/key}
			
			<!-- Left Button Overlay -->
			<button
				onclick={prevSlide}
				class="absolute left-4 top-1/2 z-20 -translate-y-1/2 flex h-16 w-16 items-center justify-center rounded-full {theme === 'onepiece' ? 'bg-chart-4/90 hover:bg-chart-2 border-2 border-chart-2' : 'bg-chart-1/30 hover:bg-chart-1/50 border-2 border-chart-1'} backdrop-blur-sm transition-all hover:scale-110 active:scale-95"
				aria-label="Previous character"
			>
				<svg class="h-8 w-8 {theme === 'onepiece' ? 'text-white' : 'text-chart-1'}" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M15 19l-7-7 7-7" />
				</svg>
			</button>
		</div>

		<!-- Center Card (Full Visible) -->
		<div class="relative w-1/2 overflow-hidden">
			{#key currentIndex}
				<div
					class="absolute inset-0"
					in:scale={{ start: 0.9, duration: 500, easing: cubicOut }}
					out:scale={{ start: 0.9, duration: 500, easing: cubicOut }}
				>
					<img
						src={characters[currentIndex].image}
						alt={characters[currentIndex].name}
						class="h-full w-full object-cover"
					/>
					<div class="absolute inset-0 bg-linear-to-t from-black/80 via-transparent to-transparent"></div>
					
					<!-- Character Info -->
					<div class="absolute bottom-0 left-0 right-0 p-12 text-center">
						<h2 class="{theme === 'onepiece' ? 'op-text text-chart-2' : 'aot-text text-chart-1'} text-6xl font-black drop-shadow-2xl mb-4">
							{characters[currentIndex].name}
						</h2>
						<p class="text-2xl font-bold text-white drop-shadow-lg mb-6">
							{characters[currentIndex].role}
						</p>
						
						<!-- Dots Indicator -->
						<div class="flex justify-center gap-2">
							{#each characters as _, index}
								<button
									onclick={() => {
										direction = index > currentIndex ? 1 : -1;
										currentIndex = index;
									}}
									class="h-2 rounded-full transition-all {index === currentIndex 
										? (theme === 'onepiece' ? 'bg-chart-2 w-12' : 'bg-chart-1 w-12')
										: 'bg-white/40 hover:bg-white/60 w-2'}"
									aria-label={`Go to character ${index + 1}`}
								></button>
							{/each}
						</div>
					</div>
				</div>
			{/key}
		</div>

		<!-- Right Side Card (Half Visible) -->
		<div class="relative w-1/4 overflow-hidden">
			{#key currentIndex}
				<div
					class="absolute inset-0"
					in:fly={{ x: direction > 0 ? 300 : -300, duration: 500, easing: cubicOut }}
					out:fly={{ x: direction > 0 ? -300 : 300, duration: 500, easing: cubicOut }}
				>
					<img
						src={characters[getCardIndex(1)].image}
						alt={characters[getCardIndex(1)].name}
						class="h-full w-full object-cover brightness-75"
					/>
					<div class="absolute inset-0 bg-black/40"></div>
					<div class="absolute bottom-8 left-4 right-4">
						<h3 class="text-2xl font-bold text-white drop-shadow-lg">
							{characters[getCardIndex(1)].name}
						</h3>
						<p class="text-sm text-white/80 drop-shadow">
							{characters[getCardIndex(1)].role}
						</p>
					</div>
				</div>
			{/key}
			
			<!-- Right Button Overlay -->
			<button
				onclick={nextSlide}
				class="absolute right-4 top-1/2 z-20 -translate-y-1/2 flex h-16 w-16 items-center justify-center rounded-full {theme === 'onepiece' ? 'bg-chart-4/90 hover:bg-chart-2 border-2 border-chart-2' : 'bg-chart-1/30 hover:bg-chart-1/50 border-2 border-chart-1'} backdrop-blur-sm transition-all hover:scale-110 active:scale-95"
				aria-label="Next character"
			>
				<svg class="h-8 w-8 {theme === 'onepiece' ? 'text-white' : 'text-chart-1'}" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M9 5l7 7-7 7" />
				</svg>
			</button>
		</div>
	</div>

	<!-- Divider Lines -->
	<div class="pointer-events-none absolute top-0 bottom-0 left-1/4 w-px bg-white/20"></div>
	<div class="pointer-events-none absolute top-0 bottom-0 right-1/4 w-px bg-white/20"></div>
</div>

<style>
	.op-text {
		font-family: 'onepiece', sans-serif;
	}

	.aot-text {
		font-family: 'Attackontitan', sans-serif;
	}
</style>
