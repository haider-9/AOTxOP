<script lang="ts">
	import { House, Users, BookOpen, Film, Globe } from '@lucide/svelte';
	import type { Icon } from '@lucide/svelte';
	import {
		Select,
		SelectContent,
		SelectGroup,
		SelectItem,
		SelectLabel,
		SelectTrigger
	} from '../ui/select';
	import { Button } from '../ui/button';

	type NavLinks = {
		name: string;
		link: string;
		icon: typeof Icon;
	};

	const navLinks: NavLinks[] = [
		{ name: 'Home', link: '/', icon: House },
		{ name: 'Characters', link: '/characters', icon: Users },
		{ name: 'Manga', link: '/manga', icon: BookOpen },
		{ name: 'Anime', link: '/anime', icon: Film }
	];

	const languages = [
		{ name: 'English', code: 'en' },
		{ name: 'Japanese', code: 'ja' },
		{ name: 'French', code: 'fr' }
	];

	let currentLanguage = $state('en');
	const trigger = $derived(
		languages.find((lang) => lang.code === currentLanguage)?.name ?? 'Select Language'
	);
</script>

<!-- GRID NAVBAR -->
<div class="grid grid-cols-3 items-center px-6 py-4">

	<!-- LEFT SIDE (Nav Links) -->
	<div class="flex justify-start">
		<nav class="rounded-full border px-2 py-1">
			<ul class="flex items-center gap-1 p-1">
				{#each navLinks as navLink}
					<li>
						<a
							href={navLink.link}
							class="flex items-center gap-2 rounded-full px-4 py-2 transition-all duration-300 hover:bg-white"
						>
							<navLink.icon class="h-5 w-5" />
							<span class="font-medium">{navLink.name}</span>
						</a>
					</li>
				{/each}
			</ul>
		</nav>
	</div>

	<!-- CENTER (Logo + Title) -->
	<div class="flex justify-center items-center gap-3">
		<div class="size-10">
			<img
				src="logoo.png"
				alt="logo"
				class="h-full w-full object-contain"
			/>
		</div>
		<h2 class="text-lg font-bold tracking-wide">
			Attack On Titan
		</h2>
	</div>

	<!-- RIGHT SIDE (Language + Button) -->
	<div class="flex justify-end items-center gap-3">
		<Select type="single" name="Language" bind:value={currentLanguage}>
			<SelectTrigger class="w-32 flex items-center gap-2">
				<Globe class="h-4 w-4" />
				{trigger}
			</SelectTrigger>

			<SelectContent>
				<SelectGroup>
					<SelectLabel>Languages</SelectLabel>
					{#each languages as language}
						<SelectItem value={language.code}>
							{language.name}
						</SelectItem>
					{/each}
				</SelectGroup>
			</SelectContent>
		</Select>

		<Button variant="outline">
			Login/Register
		</Button>
	</div>

</div>
