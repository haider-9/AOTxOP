<script lang="ts">
	import CharacterSlider from '../character-slider.svelte';

	const onePieceCharacters = [
		{ name: 'Monkey D. Luffy', role: 'Captain of the Straw Hats', image: '/onepiece/luuffy.jpg' },
		{ name: 'Roronoa Zoro', role: 'Swordsman', image: '/onepiece/zoro.png' },
		{ name: 'Nami', role: 'Navigator', image: '/onepiece/nami.jpg' },
		{ name: 'Sanji', role: 'Cook', image: '/onepiece/sanji.jpg' },
		{ name: 'Tony Tony Chopper', role: 'Doctor', image: '/onepiece/chopper.png' },
		{ name: 'Trafalgar Law', role: 'Surgeon of Death', image: '/onepiece/law.webp' },
		{ name: 'Boa Hancock', role: 'Pirate Empress', image: '/onepiece/boa.png' }
	];

	const aotCharacters = [
		{ name: 'Eren Yeager', role: 'Attack Titan', image: '/attackontitan/ere.jpg' },
		{ name: 'Levi Ackerman', role: 'Captain', image: '/attackontitan/levi.png' },
		{ name: 'Armin Arlert', role: 'Colossal Titan', image: '/attackontitan/armin.jpg' },
		{ name: 'Erwin Smith', role: 'Commander', image: '/attackontitan/erwin.jpg' },
		{ name: 'Hange Zoë', role: 'Squad Leader', image: '/attackontitan/hange.jpg' },
		{ name: 'Jean Kirstein', role: 'Survey Corps', image: '/attackontitan/jean.jpg' },
		{ name: 'Sasha Blouse', role: 'Survey Corps', image: '/attackontitan/sasha.webp' },
		{ name: 'Connie Springer', role: 'Survey Corps', image: '/attackontitan/connie.png' }
	];
</script>

<div class="min-h-screen bg-linear-to-b from-background to-muted/20">
	<!-- One Piece Section -->
	<section class="relative py-16">
		<div class="absolute inset-0 bg-linear-to-b from-chart-4/10 to-transparent pointer-events-none"></div>
		<div class="container mx-auto px-4">
			<h2 class="op-text text-center text-5xl font-black text-chart-2 drop-shadow-lg mb-8">
				STRAW HAT CREW
			</h2>
			<CharacterSlider characters={onePieceCharacters} theme="onepiece" />
		</div>
	</section>

	<!-- Divider -->
	<div class="relative h-px bg-linear-to-r from-transparent via-muted-foreground/30 to-transparent my-16"></div>

	<!-- Attack on Titan Section -->
	<section class="relative py-16">
		<div class="absolute inset-0 bg-linear-to-b from-chart-1/10 to-transparent pointer-events-none"></div>
		<div class="container mx-auto px-4">
			<h2 class="aot-text text-center text-5xl font-black text-chart-1 drop-shadow-lg mb-8">
				SURVEY CORPS
			</h2>
			<CharacterSlider characters={aotCharacters} theme="aot" />
		</div>
	</section>
</div>

<style>
	.op-text {
		font-family: 'onepiece', sans-serif;
	}

	.aot-text {
		font-family: 'Attackontitan', sans-serif;
	}
</style>