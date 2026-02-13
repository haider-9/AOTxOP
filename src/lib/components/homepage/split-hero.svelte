<script lang="ts">
	import { Button } from '../ui/button';
	import Ash from '../ash.svelte';
	import Confetti from '../confetti.svelte';

	type NavLink = {
		name: string;
		href: string;
	};

	const opNavLinks: NavLink[] = [
		{ name: 'Crew', href: '/one-piece/crew' },
		{ name: 'Episodes', href: '/one-piece/episodes' },
		{ name: 'Arcs', href: '/one-piece/arcs' },
		{ name: 'Devil Fruits', href: '/one-piece/devil-fruits' },
		{ name: 'Bounties', href: '/one-piece/bounties' }
	];

	const aotNavLinks: NavLink[] = [
		{ name: 'Survey Corps', href: '/aot/survey-corps' },
		{ name: 'Episodes', href: '/aot/episodes' },
		{ name: 'Titans', href: '/aot/titans' },
		{ name: 'Characters', href: '/aot/characters' },
		{ name: 'Timeline', href: '/aot/timeline' }
	];

	let hoveredSide = $state<'op' | 'aot' | null>(null);
</script>

<div class="relative flex h-screen w-full overflow-hidden">
	<!-- One Piece Side - Cartoonish Pirate Theme -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div
		class="relative cursor-pointer overflow-hidden transition-all duration-300 ease-out {hoveredSide === 'op' ? 'w-[60%]' : hoveredSide === 'aot' ? 'w-[40%]' : 'w-1/2'}"
		onmouseenter={() => (hoveredSide = 'op')}
		onmouseleave={() => (hoveredSide = null)}
	>
		<img
			src="/one-piece-banner.jpg"
			alt=""
			class="absolute inset-0 h-full w-full object-cover transition-transform duration-300 {hoveredSide === 'op' ? 'scale-105' : ''}"
		/>
		{#if hoveredSide === 'op'}
			<Confetti />
		{/if}

		<div class="relative z-10 flex h-full flex-col justify-between p-8">
			<!-- One Piece Nav - Cartoonish Strip -->
			<nav class="transition-opacity duration-200 {hoveredSide === 'op' ? 'opacity-100' : 'opacity-0'}">
				<div class="op-nav-strip inline-flex items-center gap-3 rounded-full border-4 border-chart-2 bg-linear-to-r from-chart-4 via-destructive to-chart-4 px-6 py-3 shadow-2xl">
					<img src="/onelogo.png" alt="One Piece" class="h-10 w-auto drop-shadow-lg" />
					<div class="h-8 w-px bg-chart-2/50"></div>
					{#each opNavLinks as link}
						<a 
							href={link.href} 
							class="op-text relative px-3 py-1.5 text-base font-bold text-white transition-all group"
						>
							<span class="relative z-10 drop-shadow-md">{link.name}</span>
							<span class="absolute inset-0 -rotate-1 rounded-lg bg-chart-2/0 transition-all group-hover:bg-chart-2/30 group-hover:rotate-0"></span>
						</a>
					{/each}
				</div>
			</nav>

			<!-- Center Title -->
			<div class="flex flex-1 items-center justify-center">
				<h1 class="op-text text-7xl font-black tracking-wider text-chart-2 drop-shadow-2xl transition-transform duration-300 {hoveredSide === 'op' ? 'scale-110' : ''}">
					ONE PIECE
				</h1>
			</div>

			<!-- Bottom CTA -->
			<div class="transition-all duration-200 {hoveredSide === 'op' ? 'translate-y-0 opacity-100' : 'translate-y-4 opacity-0'}">
				<Button class="op-text relative overflow-hidden rounded-xl border-4 border-chart-2 bg-chart-4 px-10 py-7 text-xl font-bold text-white shadow-2xl transition-all hover:scale-105 hover:-rotate-1">
					Set Sail ⚓
				</Button>
			</div>
		</div>
	</div>

	<!-- Attack on Titan Side - Metallic Military Theme -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div
		class="relative cursor-pointer overflow-hidden transition-all duration-300 ease-out {hoveredSide === 'aot' ? 'w-[60%]' : hoveredSide === 'op' ? 'w-[40%]' : 'w-1/2'}"
		onmouseenter={() => (hoveredSide = 'aot')}
		onmouseleave={() => (hoveredSide = null)}
	>
		<img
			src="/aotbanner.jpg"
			alt=""
			class="absolute inset-0 h-full w-full object-cover transition-transform duration-300 {hoveredSide === 'aot' ? 'scale-105' : ''}"
		/>
		{#if hoveredSide === 'aot'}
			<Ash />
		{/if}

		<div class="relative z-10 flex h-full flex-col justify-between p-8">
			<!-- AOT Nav - Metallic Strip -->
			<nav class="transition-opacity duration-200 {hoveredSide === 'aot' ? 'opacity-100' : 'opacity-0'}">
				<div class="border flex items-center gap-3 w-fit p-3 justify-around bg-white/10 backdrop-blur-3xl">
					<img src="/logoo.png" alt="AOT" class="h-10 w-auto brightness-110 contrast-125" />
					<div class="h-8 w-px bg-chart-1/30"></div>
					{#each aotNavLinks as link}
						<a 
							href={link.href} 
							class="aot-text group relative px-3 py-1.5 text-xs font-bold uppercase tracking-widest text-background transition-all hover:text-chart-1"
						>
							<span class="relative z-10">{link.name}</span>
							<span class="absolute bottom-0 left-0 h-px w-0 bg-chart-1 transition-all duration-300 group-hover:w-full"></span>
						</a>
					{/each}
				</div>
			</nav>

			<!-- Center Title -->
			<div class="flex flex-1 items-center justify-center">
				<h1 class="aot-text text-6xl font-black tracking-wider text-chart-1 drop-shadow-2xl transition-transform duration-300 {hoveredSide === 'aot' ? 'scale-110' : ''}">
					ATTACK ON TITAN
				</h1>
			</div>

			<!-- Bottom CTA -->
			<div class="transition-all duration-200 {hoveredSide === 'aot' ? 'translate-y-0 opacity-100' : 'translate-y-4 opacity-0'}">
				<Button class="aot-text group relative overflow-hidden border border-chart-1/50 bg-linear-to-b from-muted to-background px-10 py-7 text-base font-bold uppercase tracking-widest text-chart-1 backdrop-blur-sm transition-all hover:border-chart-1 hover:bg-chart-1/10">
					<span class="relative z-10 flex items-center gap-2">
						Deploy Now
					</span>
				</Button>
			</div>
		</div>
	</div>

	<!-- Center Line -->
	<div class="pointer-events-none absolute top-0 bottom-0 w-px bg-white/20 transition-all duration-300 {hoveredSide === 'op' ? 'left-[60%]' : hoveredSide === 'aot' ? 'left-[40%]' : 'left-1/2'}"></div>
</div>

<style>
	/* One Piece Cartoonish Nav */
	.op-nav-strip {
		box-shadow: 
			0 4px 0 rgba(0, 0, 0, 0.3),
			0 8px 20px rgba(0, 0, 0, 0.5),
			inset 0 2px 0 rgba(255, 255, 255, 0.3);
		transform: rotate(-0.5deg);
	}


	@media (max-width: 768px) {
		.relative.h-screen {
			flex-direction: column;
		}
	}
</style>
